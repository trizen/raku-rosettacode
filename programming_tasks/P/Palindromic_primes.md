[1]: https://rosettacode.org/wiki/Palindromic_primes

# [Palindromic primes][1]

```perl
say "{+$_} matching numbers:\n{.batch(10)».fmt('%3d').join: "\n"}"
    given (^1000).grep: { .is-prime and $_ eq .flip };
```

#### Output:
```
20 matching numbers:
  2   3   5   7  11 101 131 151 181 191
313 353 373 383 727 757 787 797 919 929
```
