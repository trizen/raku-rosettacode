[1]: http://rosettacode.org/wiki/Negative_base_numbers

# [Negative base numbers][1]

Perl 6 provides built-in methods / routines base and parse-base to convert to and from bases 2 through 36. We'll just shadow the core routines with versions that accept negative bases. We'll simplify things by only accepting integer values to convert.

```perl
multi sub base (Int $value, Int $radix where -37 < * < -1) {
    my @result = $value.polymod($radix xx *);
    for @result.kv -> $k, $v {
        if $v < 0 {
            @result[$k    ] -= $radix;
            @result[$k + 1] += 1;
        }
    }
    @result.pop while @result.tail == 0;
    @result».=base($radix.abs);
    flip [~] @result;
}
 
multi sub parse-base (Str $str, Int $radix where -37 < * < -1) {
    [+] $str.flip.comb.kv.map: { $^v.parse-base($radix.abs) * $radix ** $^k }
}
 
# TESTING
say '           10.&base( -2) = ', 10.&base(-2);
say '          146.&base( -3) = ', 146.&base(-3);
say '           15.&base(-10) = ', 15.&base(-10);
say '          107.&base(-16) = ', 107.&base(-16);
say '     41371458.&base(-36) = ', 41371458.&base(-36);
 
say '"11110".&parse-base( -2) = ', "11110".&parse-base(-2);
say '"21102".&parse-base( -3) = ', "21102".&parse-base(-3);
say '  "195".&parse-base(-10) = ', "195".&parse-base(-10);
say '  "1AB".&parse-base(-16) = ', "1AB".&parse-base(-16);
say '"Perl6".&parse-base(-36) = ', "Perl6".&parse-base(-36);
```

#### Output:
```
           10.&base( -2) = 11110
          146.&base( -3) = 21102
           15.&base(-10) = 195
          107.&base(-16) = 1AB
     41371458.&base(-36) = PERL6
"11110".&parse-base( -2) = 10
"21102".&parse-base( -3) = 146
  "195".&parse-base(-10) = 15
  "1AB".&parse-base(-16) = 107
"Perl6".&parse-base(-36) = 41371458
```