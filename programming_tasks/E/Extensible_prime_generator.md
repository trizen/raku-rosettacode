[1]: https://rosettacode.org/wiki/Extensible_prime_generator

# [Extensible prime generator][1]


Build a lazy infinite list of primes using the Raku builtin is-prime method. ( A lazy list will not bother to calculate the values until they are actually used. ) That is the first line. If you choose to calculate the entire list, it is fairly certain that you will run out of process / system memory before you finish... (patience too probably) but there aren't really any other constraints. The is-prime builtin uses a Miller-Rabin primality test with 100 iterations, so while it is not 100% absolutely guaranteed that every number it flags as prime actually is, the chances that it is wrong are about 4<sup>-100</sup>. Much less than the chance that a cosmic ray will cause an error in your computers CPU.  Everything after the first line is just display code for the various task requirements.

```perl
my @primes = lazy gather for 1 .. * { .take if .is-prime }

say "The first twenty primes:\n   ", "[{@primes[^20].fmt("%d", ', ')}]";
say "The primes between 100 and 150:\n   ", "[{@primes.&between(100, 150).fmt("%d", ', ')}]";
say "The number of primes between 7,700 and 8,000:\n   ", +@primes.&between(7700, 8000);
say "The 10,000th prime:\n   ", @primes[9999];

sub between (@p, $l, $u) {
    gather for @p { .take if $l < $_ < $u; last if $_ >= $u }
}
```

#### Output:
```
The first twenty primes:
   [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71]
The primes between 100 and 150:
   [101, 103, 107, 109, 113, 127, 131, 137, 139, 149]
The number of primes between 7,700 and 8,000:
   30
The 10,000th prime:
   104729
```
