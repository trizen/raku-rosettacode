[1]: https://rosettacode.org/wiki/Largest_difference_between_adjacent_primes

# [Largest difference between adjacent primes][1]

### Built-ins

```perl
for 2..8 -> $n {
    printf "Largest prime gap up to {10 ** $n}: %d - between %d and %d.\n", .[0], |.[1]
      given max (^10**$n).grep(&is-prime).rotor(2=>-1).map({.[1]-.[0],$_})
}
```

#### Output:
```
Largest prime gap up to 100: 8 - between 89 and 97.
Largest prime gap up to 1000: 20 - between 887 and 907.
Largest prime gap up to 10000: 36 - between 9551 and 9587.
Largest prime gap up to 100000: 72 - between 31397 and 31469.
Largest prime gap up to 1000000: 114 - between 492113 and 492227.
Largest prime gap up to 10000000: 154 - between 4652353 and 4652507.
Largest prime gap up to 100000000: 220 - between 47326693 and 47326913.
```


Or, significantly faster using a



### Module

```perl
use Math::Primesieve;
my $sieve = Math::Primesieve.new;
 
for 2..8 -> $n {
    printf "Largest prime gap up to {10 ** $n}: %d - between %d and %d.\n", .[0], |.[1]
      given max $sieve.primes(10 ** $n).rotor(2=>-1).map({.[1]-.[0],$_})
}
```


Same output
