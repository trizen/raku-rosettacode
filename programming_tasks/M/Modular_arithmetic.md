[1]: https://rosettacode.org/wiki/Modular_arithmetic

# [Modular arithmetic][1]





We'll use the [FiniteFields](https://raku.land/github:grondilu/finite-fields-raku) repo.

```perl
use FiniteField;
$*modulus = 13;

sub f(\x) { x**100 + x + 1};

say f(10);
```

#### Output:
```
1
```
