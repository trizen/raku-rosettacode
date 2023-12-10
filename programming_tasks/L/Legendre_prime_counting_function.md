[1]: https://rosettacode.org/wiki/Legendre_prime_counting_function

# [Legendre prime counting function][1]

Seems like an awful lot of pointless work. Using prime sieve anyway, why not just use it?

```perl
use Math::Primesieve;

my $sieve = Math::Primesieve.new;

say "10^$_\t" ~ $sieve.count: exp($_,10) for ^10;

say (now - INIT now) ~ ' elapsed seconds';
```

#### Output:
```
10^0    0
10^1    4
10^2    25
10^3    168
10^4    1229
10^5    9592
10^6    78498
10^7    664579
10^8    5761455
10^9    50847534
0.071464489 elapsed seconds
```
