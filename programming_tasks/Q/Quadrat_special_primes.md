[1]: https://rosettacode.org/wiki/Quadrat_special_primes

# [Quadrat special primes][1]

```perl
my @sqp = 2, -> $previous {
    my $next;
    for (1..∞).map: *² {
        $next = $previous + $_;
        last if $next.is-prime;
    }
    $next
} … *;
 
say "{+$_} matching numbers:\n", $_».fmt('%5d').batch(7).join: "\n" given
    @sqp[^(@sqp.first: * > 16000, :k)];
```

#### Output:
```
49 matching numbers:
    2     3     7    11    47    83   227
  263   587   911   947   983  1019  1163
 1307  1451  1487  1523  1559  2459  3359
 4259  4583  5483  5519  5843  5879  6203
 6779  7103  7247  7283  7607  7643  8219
 8363 10667 11243 11279 11423 12323 12647
12791 13367 13691 14591 14627 14771 15671
```
