[1]: https://rosettacode.org/wiki/Piprimes

# [Piprimes][1]

```perl
my @pi = (1..*).map: { state $pi = 0; $pi += .is-prime };
 
say @pi[^(@pi.first: * >= 22, :k)].batch(10)».fmt('%2d').join: "\n";
```

#### Output:
```
 0  1  2  2  3  3  4  4  4  4
 5  5  6  6  6  6  7  7  8  8
 8  8  9  9  9  9  9  9 10 10
11 11 11 11 11 11 12 12 12 12
13 13 14 14 14 14 15 15 15 15
15 15 16 16 16 16 16 16 17 17
18 18 18 18 18 18 19 19 19 19
20 20 21 21 21 21 21 21
```
