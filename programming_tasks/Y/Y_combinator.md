[1]: https://rosettacode.org/wiki/Y_combinator

# [Y combinator][1]



```perl
sub Y (&f) { sub (&x) { x(&x) }( sub (&y) { f(sub ($x) { y(&y)($x) }) } ) }
sub fac (&f) { sub ($n) { $n < 2 ?? 1 !! $n * f($n - 1) } }
sub fib (&f) { sub ($n) { $n < 2 ?? $n !! f($n - 1) + f($n - 2) } }
say map Y($_), ^10 for &fac, &fib;
```

#### Output:
```
(1 1 2 6 24 120 720 5040 40320 362880)
(0 1 1 2 3 5 8 13 21 34)
```


Note that Raku doesn't actually need a Y combinator because you can name anonymous functions from the inside:

```perl
say .(10) given sub (Int $x) { $x < 2 ?? 1 !! $x * &?ROUTINE($x - 1); }
```
