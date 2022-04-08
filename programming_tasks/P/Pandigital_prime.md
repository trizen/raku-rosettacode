[1]: https://rosettacode.org/wiki/Pandigital_prime

# [Pandigital prime][1]

```perl
for 1, 0 -> $i {
    say max ($i..7).map: -> $size {
        |($i..$size).permutations».join.grep(&is-prime);
    }
}
```

#### Output:
```
7652413
76540231
```
