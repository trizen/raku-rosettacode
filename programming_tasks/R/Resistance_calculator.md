[1]: https://rosettacode.org/wiki/Resistance_calculator

# [Resistance calculator][1]





### Infix

```perl
class Resistor {
    has Str        $.symbol;
    has Numeric  ( $.voltage, $.resistance );
    has Resistor ( $.a, $.b );

    method res ( ) {
        given $.symbol {
            when '+' { return $.a.res + $.b.res }
            when '*' { return 1 / (1 / $.a.res  +  1 / $.b.res) }
            default  { return $.resistance }
        }
    }

    method set-voltage ( Numeric $voltage ) {
        given $.symbol {
            when '+' {
                my $ra = $.a.res;
                my $rb = $.b.res;
                $.a.set-voltage( $ra / ($ra+$rb) * $voltage );
                $.b.set-voltage( $rb / ($ra+$rb) * $voltage );
            }
            when '*' {
                $.a.set-voltage( $voltage );
                $.b.set-voltage( $voltage );
            }
        }
        $!voltage = $voltage;
    }
    method current ( ) { return $.voltage / self.res     }
    method effect  ( ) { return $.voltage * self.current }

    method report ( Int $level = 1 ) {
        my $pad = '| ' x $level;
        my $f = ( self.res, $.voltage, self.current, self.effect ).fmt('%8.3f');
        say "$f $pad$.symbol";
        $.a.report( $level+1 ) if $.a;
        $.b.report( $level+1 ) if $.b;
    }
}
multi sub infix:<+> (Resistor $a, Resistor $b) { $a.new( symbol => '+', :$a, :$b ) }
multi sub infix:<*> (Resistor $a, Resistor $b) { $a.new( symbol => '*', :$a, :$b ) }

my Resistor ($R1, $R2, $R3, $R4, $R5, $R6, $R7, $R8, $R9, $R10) =
    map { Resistor.new: symbol => 'r', resistance => $_ },
    6, 8, 4, 8, 4, 6, 8, 10, 6, 2;

my $node = (((($R8 + $R10) * $R9 + $R7) * $R6 + $R5)
                           * $R4 + $R3) * $R2 + $R1;
$node.set-voltage(18);

say '     Ohm     Volt   Ampere     Watt  Network tree';
$node.report;
```

#### Output:
```
     Ohm     Volt   Ampere     Watt  Network tree
  10.000   18.000    1.800   32.400 | +
   4.000    7.200    1.800   12.960 | | *
   8.000    7.200    0.900    6.480 | | | +
   4.000    3.600    0.900    3.240 | | | | *
   8.000    3.600    0.450    1.620 | | | | | +
   4.000    1.800    0.450    0.810 | | | | | | *
  12.000    1.800    0.150    0.270 | | | | | | | +
   4.000    0.600    0.150    0.090 | | | | | | | | *
  12.000    0.600    0.050    0.030 | | | | | | | | | +
  10.000    0.500    0.050    0.025 | | | | | | | | | | r
   2.000    0.100    0.050    0.005 | | | | | | | | | | r
   6.000    0.600    0.100    0.060 | | | | | | | | | r
   8.000    1.200    0.150    0.180 | | | | | | | | r
   6.000    1.800    0.300    0.540 | | | | | | | r
   4.000    1.800    0.450    0.810 | | | | | | r
   8.000    3.600    0.450    1.620 | | | | | r
   4.000    3.600    0.900    3.240 | | | | r
   8.000    7.200    0.900    6.480 | | | r
   6.000   10.800    1.800   19.440 | | r
```
