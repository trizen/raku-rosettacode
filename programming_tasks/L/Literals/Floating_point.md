[1]: https://rosettacode.org/wiki/Literals/Floating_point

# [Literals/Floating point][1]


Floating point numbers (the Num type) are written in the standard 'e' scientific notation:

```perl
2e2      # same as 200e0, 2e2, 200.0e0  and 2.0e2
6.02e23
-2e48
1e-9
1e0
```


A number like `3.1416` is specifically not floating point, but rational (the Rat type), equivalent to `3927/1250`.  On the other hand, `Num(3.1416)` would be considered a floating literal though by virtue of mandatory constant folding.
