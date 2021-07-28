[1]: https://rosettacode.org/wiki/Wieferich_primes

# [Wieferich primes][1]

```perl
put "Wieferich primes less than 5000: ", join ', ', ^5000 .grep: { .is-prime and not ( exp($_-1, 2) - 1 ) % .² };
```

#### Output:
```
Wieferich primes less than 5000: 1093, 3511
```
