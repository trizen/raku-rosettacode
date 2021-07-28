[1]: https://rosettacode.org/wiki/Palindromic_primes_in_base_16

# [Palindromic primes in base 16][1]

Trivial modification of [Palindromic primes](https://rosettacode.org/wiki/Palindromic_primes#Raku) task.

```perl
say "{+$_} matching numbers:\n{.batch(10)».fmt('%3X').join: "\n"}"
    given (^500).grep: { .is-prime and .base(16) eq .base(16).flip };
```

#### Output:
```
13 matching numbers:
  2   3   5   7   B   D  11 101 151 161
191 1B1 1C1
```
