[1]: https://rosettacode.org/wiki/Repunit_primes

# [Repunit primes][1]

```perl
my $limit = 2700;

say "Repunit prime digits (up to $limit) in:";

.put for (2..16).hyper(:1batch).map: -> $base {
    $base.fmt("Base %2d: ") ~ (1..$limit).grep(&is-prime).grep( (1 x *).parse-base($base).is-prime )
}
```

#### Output:
```
Repunit prime digits (up to 2700) in:
Base  2: 2 3 5 7 13 17 19 31 61 89 107 127 521 607 1279 2203 2281
Base  3: 3 7 13 71 103 541 1091 1367 1627
Base  4: 2
Base  5: 3 7 11 13 47 127 149 181 619 929
Base  6: 2 3 7 29 71 127 271 509 1049
Base  7: 5 13 131 149 1699
Base  8: 3
Base  9: 
Base 10: 2 19 23 317 1031
Base 11: 17 19 73 139 907 1907 2029
Base 12: 2 3 5 19 97 109 317 353 701
Base 13: 5 7 137 283 883 991 1021 1193
Base 14: 3 7 19 31 41 2687
Base 15: 3 43 73 487 2579
Base 16: 2
```


*And for my own amusement, also tested up to 2700.*


```
Base 17: 3 5 7 11 47 71 419
Base 18: 2
Base 19: 19 31 47 59 61 107 337 1061
Base 20: 3 11 17 1487
Base 21: 3 11 17 43 271
Base 22: 2 5 79 101 359 857
Base 23: 5
Base 24: 3 5 19 53 71 653 661
Base 25: 
Base 26: 7 43 347
Base 27: 3
Base 28: 2 5 17 457 1423
Base 29: 5 151
Base 30: 2 5 11 163 569 1789
Base 31: 7 17 31
Base 32: 
Base 33: 3 197
Base 34: 13 1493
Base 35: 313 1297
Base 36: 2
```
