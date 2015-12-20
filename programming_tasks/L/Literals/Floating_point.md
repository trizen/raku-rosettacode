[1]: http://rosettacode.org/wiki/Literals/Floating_point

# [Literals/Floating point][1]

Floating point numbers (the Num type) are written various forms of scientific notation:

```perl
6.02e23                   # standard E notation
:10<6.02 * 10 ** 23>      # radix notation
:5<11.002 * 10 ** 23>     # exponent is still decimal
:5<11.002*:5<20>**:5<43>> # all in base 5
```


A number like `3.1416` is specifically not floating point, but rational (the Rat type), equivalent to `3927/1250`. On the other hand, `Num(3.1416)` would be considered a floating literal though by virtue of mandatory constant folding (not yet implemented).