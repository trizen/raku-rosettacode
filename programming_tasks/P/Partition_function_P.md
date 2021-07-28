[1]: https://rosettacode.org/wiki/Partition_function_P

# [Partition function P][1]

Not the fastest, but it gets the job done.

```perl
my @P = 1, { p(++$) } … *;
my @i = lazy [\+] flat 1, ( (1 .. *) Z (1 .. *).map: * × 2 + 1 );
sub p ($n) { sum @P[$n X- @i[^(@i.first: * > $n, :k)]] Z× (flat (1, 1, -1, -1) xx *) }
 
put @P[^26];
put @P[6666];
```

#### Output:
```
1 1 2 3 5 7 11 15 22 30 42 56 77 101 135 176 231 297 385 490 627 792 1002 1255 1575 1958
193655306161707661080005073394486091998480950338405932486880600467114423441282418165863
```
