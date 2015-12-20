[1]: http://rosettacode.org/wiki/Modular_arithmetic

# [Modular arithmetic][1]

There is a Panda module called Modular which works basically as Perl 5's Math::ModInt.

```perl
use Modular;
sub f(\x) { x**100 + x + 1};
say f( 10 Mod 13 )
```

#### Output:
```
1 「mod 13」
```