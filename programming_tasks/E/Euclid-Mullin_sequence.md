[1]: https://rosettacode.org/wiki/Euclid-Mullin_sequence

# [Euclid-Mullin sequence][1]

```perl
use Prime::Factor;

my @Euclid-Mullin = 2, { state $i = 1; (1 + [×] @Euclid-Mullin[^$i++]).&prime-factors.min } … *;

put 'First sixteen: ', @Euclid-Mullin[^16];
```

#### Output:
```
First sixteen: 2 3 7 43 13 53 5 6221671 38709183810571 139 2801 11 17 5471 52662739 23003
```
