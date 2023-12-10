[1]: https://rosettacode.org/wiki/Euler%27s_identity

# [Euler&#039;s identity][1]





Implementing an "invisible times" operator (Unicode character (U+2062)) to more closely emulate the layout. Alas, Raku does not do symbolic calculations at this time and is limited to IEEE 754 floating point for transcendental and irrational number calculations.



e, i and π are all available as built-in constants in Raku.

```perl
sub infix:<⁢> is tighter(&infix:<**>) { $^a * $^b };

say 'e**i⁢π + 1 ≅ 0 : ', e**i⁢π + 1 ≅ 0;
say 'Error: ', e**i⁢π + 1;
```

#### Output:
```
e**i⁢π + 1 ≅ 0 : True
Error: 0+1.2246467991473532e-16i
```
