[1]: https://rosettacode.org/wiki/Sum_of_primes_in_odd_positions_is_prime

# [Sum of primes in odd positions is prime][1]

```perl
my @odd  = grep { ++$ !%% 2 }, grep &is-prime, 2 ..^ 1000;
my @sums = [\+] @odd;

say .fmt('%5d') for grep { .[2].is-prime }, ( (1,3…*) Z @odd Z @sums );
```

#### Output:
```
    1     2     2
    3     5     7
   11    31    89
   27   103   659
   35   149  1181
   67   331  5021
   91   467  9923
   95   499 10909
   99   523 11941
  119   653 17959
  143   823 26879
```
