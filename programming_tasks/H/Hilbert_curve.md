[1]: https://rosettacode.org/wiki/Hilbert_curve

# [Hilbert curve][1]

```perl
use SVG;
 
role Lindenmayer {
    has %.rules;
    method succ {
        self.comb.map( { %!rules{$^c} // $c } ).join but Lindenmayer(%!rules)
    }
}
 
my $hilbert = 'A' but Lindenmayer( { A => '-BF+AFA+FB-', B => '+AF-BFB-FA+' } );
 
$hilbert++ xx 7;
my @points = (647, 13);
 
for $hilbert.comb {
    state ($x, $y) = @points[0,1];
    state $d = -5 - 0i;
    when 'F' { @points.append: ($x += $d.re).round(1), ($y += $d.im).round(1) }
    when /< + - >/ { $d *= "{$_}1i" }
    default { }
}
 
say SVG.serialize(
    svg => [
        :660width, :660height, :style<stroke:blue>,
        :rect[:width<100%>, :height<100%>, :fill<white>],
        :polyline[ :points(@points.join: ','), :fill<white> ],
    ],
);
```


See: [Hilbert curve](https://github.com/thundergnat/rc/blob/master/img/hilbert-perl6.svg)



There is a variation of a Hilbert curve known as a [Moore curve](https://en.wikipedia.org/wiki/Moore_curve) which is essentially 4 Hilbert curves joined together in a loop.

```perl
use SVG;
 
role Lindenmayer {
    has %.rules;
    method succ {
        self.comb.map( { %!rules{$^c} // $c } ).join but Lindenmayer(%!rules)
    }
}
 
my $moore = 'AFA+F+AFA' but Lindenmayer( { A => '-BF+AFA+FB-', B => '+AF-BFB-FA+' } );
 
$moore++ xx 6;
my @points = (327, 647);
 
for $moore.comb {
    state ($x, $y) = @points[0,1];
    state $d = 0 - 5i;
    when 'F' { @points.append: ($x += $d.re).round(1), ($y += $d.im).round(1) }
    when /< + - >/ { $d *= "{$_}1i" }
    default { }
}
 
say SVG.serialize(
    svg => [
        :660width, :660height, :style<stroke:darkviolet>,
        :rect[:width<100%>, :height<100%>, :fill<white>],
        :polyline[ :points(@points.join: ','), :fill<white> ],
    ],
);
```


See: [Moore curve](https://github.com/thundergnat/rc/blob/master/img/moore-perl6.svg)