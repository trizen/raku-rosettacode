[1]: https://rosettacode.org/wiki/Sum_of_square_and_cube_digits_of_an_integer_are_primes

# [Sum of square and cube digits of an integer are primes][1]

```perl
say ^100 .grep: { .².comb.sum.is-prime && .³.comb.sum.is-prime }
```

#### Output:
```
(16 17 25 28 34 37 47 52 64)
```
