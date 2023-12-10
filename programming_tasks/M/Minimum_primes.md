[1]: https://rosettacode.org/wiki/Minimum_primes

# [Minimum primes][1]

Seems kind of pointless to specify a maximum of 5 terms when there are only 5 elements in each list but... ¯\\_(ツ)\_/¯

```perl
say ([Zmax] <5 45 23 21 67>, <43 22 78 46 38>, <9 98 12 54 53>)».&next-prime[^5];

sub next-prime { ($^m..*).first: &is-prime }
```

#### Output:
```
(43 101 79 59 67)
```
