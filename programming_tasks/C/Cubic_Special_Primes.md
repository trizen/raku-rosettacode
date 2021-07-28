[1]: https://rosettacode.org/wiki/Cubic_Special_Primes

# [Cubic Special Primes][1]

A two character difference from the [Quadrat Special Primes](https://rosettacode.org/wiki/Quadrat_Special_Primes#Raku) entry. (And it could have been one.)

```perl
my @sqp = 2, -> $previous {
    my $next;
    for (1..∞).map: *³ {
        $next = $previous + $_;
        last if $next.is-prime;
    }
    $next
} … *;
 
say "{+$_} matching numbers:\n", $_».fmt('%5d').batch(7).join: "\n" given
    @sqp[^(@sqp.first: * > 15000, :k)];
```

#### Output:
```
23 matching numbers:
    2     3    11    19    83  1811  2027
 2243  2251  2467  2531  2539  3539  3547
 4547  5059 10891 12619 13619 13627 13691
13907 14419
```
