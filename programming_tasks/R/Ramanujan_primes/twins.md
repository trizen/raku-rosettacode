[1]: https://rosettacode.org/wiki/Ramanujan_primes/twins

# [Ramanujan primes/twins][1]

Timing is informational only. Will vary by system specs and load.

```perl
use ntheory:from<Perl5> <ramanujan_primes nth_ramanujan_prime>;
use Lingua::EN::Numbers;
 
my @rp = ramanujan_primes nth_ramanujan_prime 1_000_000;
 
for (1e5, 1e6)».Int -> $limit {
    say "\nThe {comma $limit}th Ramanujan prime is { comma @rp[$limit-1]}";
    say "There are { comma +(^($limit-1)).race.grep: { @rp[$_+1] == @rp[$_]+2 } } twins in the first {comma $limit} Ramanujan primes.";
}
 
say (now - INIT now).fmt('%.3f') ~ ' seconds';
```

#### Output:
```
The 100,000th Ramanujan prime is 2,916,539
There are 8,732 twins in the first 100,000 Ramanujan primes.

The 1,000,000th Ramanujan prime is 34,072,993
There are 74,973 twins in the first 1,000,000 Ramanujan primes.
2.529 seconds
```
