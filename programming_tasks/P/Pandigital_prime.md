[1]: https://rosettacode.org/wiki/Pandigital_prime

# [Pandigital prime][1]

```perl
say ($_..7).reverse.permutations».join.first: &is-prime for 1,0;
```

#### Output:
```
7652413
76540231
```
