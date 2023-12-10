[1]: https://rosettacode.org/wiki/Tropical_algebra_overloading

# [Tropical algebra overloading][1]

No need to overload, define our own operators with whatever precedence level we want. Here we're setting precedence equivalent to existing operators.

```perl
sub infix:<⊕> (Real $a, Real $b) is equiv(&[+]) { $a max $b }
sub infix:<⊗> (Real $a, Real $b) is equiv(&[×]) { $a + $b }
sub infix:<↑> (Real $a,  Int $b where * ≥ 0) is equiv(&[**]) { [⊗] $a xx $b }
 
use Test;
 
is-deeply(      2 ⊗ -2,        0, '2 ⊗ -2 == 0' );
is-deeply( -0.001 ⊕ -Inf, -0.001, '-0.001 ⊕ -Inf == -0.001' );
is-deeply(      0 ⊗ -Inf,   -Inf, '0 ⊗ -Inf == -Inf' );
is-deeply(    1.5 ⊕ -1,      1.5, '1.5 ⊕ -1 == 1.5' );
is-deeply(   -0.5 ⊗ 0,      -0.5, '-0.5 ⊗ 0 == -0.5' );
is-deeply(      5 ↑ 7,        35, '5 ↑ 7 == 35' );
is-deeply( 5 ⊗ (8 ⊕ 7),  5 ⊗ 8 ⊕ 5 ⊗ 7, '5 ⊗ (8 ⊕ 7) == 5 ⊗ 8 ⊕ 5 ⊗ 7');
is-deeply( 5 ↑ 7 ⊕ 6 ↑ 6,     36, '5 ↑ 7 ⊕ 6 ↑ 6 == 36');
```

#### Output:
```
ok 1 - 2 ⊗ -2 == 0
ok 2 - -0.001 ⊕ -Inf == -0.001
ok 3 - 0 ⊗ -Inf == -Inf
ok 4 - 1.5 ⊕ -1 == 1.5
ok 5 - -0.5 ⊗ 0 == -0.5
ok 6 - 5 ↑ 7 == 35
ok 7 - 5 ⊗ (8 ⊕ 7) == 5 ⊗ 8 ⊕ 5 ⊗ 7
ok 8 - 5 ↑ 7 ⊕ 6 ↑ 6 == 36
```
