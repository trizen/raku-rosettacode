[1]: https://rosettacode.org/wiki/Summation_of_primes

# [Summation of primes][1]

Slow, but only using compiler built-ins (about 5 seconds)

```perl
say sum (^2e6).grep: {.&is-prime};
```

#### Output:
```
142913828922
```


Much faster using external libraries (well under half a second)

```perl
use Math::Primesieve;
my $sieve = Math::Primesieve.new;
say sum $sieve.primes(2e6.Int);
```


Same output
