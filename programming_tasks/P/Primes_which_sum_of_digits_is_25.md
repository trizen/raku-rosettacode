[1]: https://rosettacode.org/wiki/Primes_which_sum_of_digits_is_25

# [Primes which sum of digits is 25][1]

```perl
unit sub MAIN ($limit = 5000);
say "{+$_} primes < $limit with digital sum 25:\n{$_».fmt("%" ~ $limit.chars ~ "d").batch(10).join("\n")}",
    with ^$limit .grep: { .is-prime and .comb.sum == 25 }
```

#### Output:
```
17 primes < 5000 with digital sum 25:
 997 1699 1789 1879 1987 2689 2797 2887 3499 3697
3769 3877 3967 4597 4759 4957 4993
```
