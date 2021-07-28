[1]: https://rosettacode.org/wiki/Approximate_equality

# [Approximate equality][1]





Is approximately equal to is a built-in operator in Raku. Unicode ≅, or the ASCII equivalent: =~=. By default it uses a tolerance of 1e-15 times the order of magnitude of the larger comparand, though that is adjustable by setting the dynamic variable $\*TOLERANCE to the desired value. Probably a good idea to localize the changed $\*TOLERANCE as it will affect all comparisons within its scope.



Most of the following tests are somewhat pointless in Raku. To a large extent, when dealing with Rational values, you don't really need to worry about "approximately equal to", and all of the test values below, with the exception of `sqrt(2)`, are Rats by default, and exact. You would have to specifically coerce them to Nums (floating point) to lose the precision.



For example, in Raku, the sum of .1, .2, .3, &amp; .4 is *identically* equal to 1.

```perl
say 0.1 + 0.2 + 0.3 + 0.4 === 1.0000000000000000000000000000000000000000000000000000000000000000000000000; # True
```


It's also *approximately* equal to 1 but... ¯\\_(ツ)\_/¯

```perl
for
    100000000000000.01, 100000000000000.011,
    100.01, 100.011,
    10000000000000.001 / 10000.0, 1000000000.0000001000,
    0.001, 0.0010000001,
    0.000000000000000000000101, 0.0,
    sqrt(2) * sqrt(2), 2.0,
    -sqrt(2) * sqrt(2), -2.0,
    100000000000000003.0, 100000000000000004.0,
    3.14159265358979323846, 3.14159265358979324
 
  -> $a, $b {
    say "$a ≅ $b: ", $a ≅ $b;
}
 
say "\nTolerance may be adjusted.";
 
say 22/7, " ≅ ", π, ": ", 22/7 ≅ π;
{ # Localize the tolerance to only this block
  my $*TOLERANCE = .001;
  say 22/7, " ≅ ", π, ": ", 22/7 ≅ π;
}
```

#### Output:
```
100000000000000.01 ≅ 100000000000000.011: True
100.01 ≅ 100.011: False
1000000000.0000001 ≅ 1000000000.0000001: True
0.001 ≅ 0.0010000001: False
0.000000000000000000000101 ≅ 0: True
2.0000000000000004 ≅ 2: True
-2.0000000000000004 ≅ -2: True
100000000000000003 ≅ 100000000000000004: True
3.141592653589793226752 ≅ 3.14159265358979324: True

Tolerance may be adjusted.
3.142857 ≅ 3.141592653589793: False
3.142857 ≅ 3.141592653589793: True
```
