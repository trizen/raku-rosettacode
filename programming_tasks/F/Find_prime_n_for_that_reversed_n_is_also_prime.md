[1]: https://rosettacode.org/wiki/Find_prime_n_for_that_reversed_n_is_also_prime

# [Find prime n for that reversed n is also prime][1]

```perl
unit sub MAIN ($limit = 500);
say "{+$_} reversible primes < $limit:\n{$_».fmt("%" ~ $limit.chars ~ "d").batch(10).join("\n")}",
    with ^$limit .grep: { .is-prime and .flip.is-prime }
```

#### Output:
```
34 reversible primes < 500:
  2   3   5   7  11  13  17  31  37  71
 73  79  97 101 107 113 131 149 151 157
167 179 181 191 199 311 313 337 347 353
359 373 383 389
```
