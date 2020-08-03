[1]: https://rosettacode.org/wiki/Euler's_identity

# [Euler's identity][1]

Implementing an "invisible times" operator (Unicode character (U+2062)) to more closely emulate the layout. Alas, Perl 6 does not do symbolic calculations at this time and is limited to IEEE 754 floating point for transcendental and irrational number calculations.



e, i and π are all available as built-in constants in Perl 6.

```raku
sub infix:<⁢> is tighter(&infix:<**>) { $^a * $^b };
 
say 'e**i⁢π + 1 ≅ 0 : ', e**i⁢π + 1 ≅ 0;
say 'Error: ', e**i⁢π + 1;
```

#### Output:
```
e**i⁢π + 1 ≅ 0 : True
Error: 0+1.2246467991473532e-16i
```