[1]: https://rosettacode.org/wiki/Pernicious_numbers

# [Pernicious numbers][1]





Straightforward implementation using Raku's *is-prime* built-in subroutine.

```perl
sub is-pernicious(Int $n --> Bool) {
    is-prime [+] $n.base(2).comb;
}

say (grep &is-pernicious, 0 .. *)[^25];
say grep &is-pernicious, 888_888_877 .. 888_888_888;
```

#### Output:
```
3 5 6 7 9 10 11 12 13 14 17 18 19 20 21 22 24 25 26 28 31 33 34 35 36
888888877 888888878 888888880 888888883 888888885 888888886
```
