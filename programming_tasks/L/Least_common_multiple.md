[1]: https://rosettacode.org/wiki/Least_common_multiple

# [Least common multiple][1]

This function is provided as an infix so that it can be used productively with various metaoperators.

```perl
say 3 lcm 4;            # infix
say [lcm] 1..20;        # reduction
say ~(1..10 Xlcm 1..10) # cross
```

#### Output:
```
12
232792560
1 2 3 4 5 6 7 8 9 10 2 2 6 4 10 6 14 8 18 10 3 6 3 12 15 6 21 24 9 30 4 4 12 4 20 12 28 8 36 20 5 10 15 20 5 30 35 40 45 10 6 6 6 12 30 6 42 24 18 30 7 14 21 28 35 42 7 56 63 70 8 8 24 8 40 24 56 8 72 40 9 18 9 36 45 18 63 72 9 90 10 10 30 20 10 30 70 40 90 10
```