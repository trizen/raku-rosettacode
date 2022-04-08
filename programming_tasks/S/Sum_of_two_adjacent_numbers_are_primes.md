[1]: https://rosettacode.org/wiki/Sum_of_two_adjacent_numbers_are_primes

# [Sum of two adjacent numbers are primes][1]

```perl
my @n-n1-triangular = map { $_, $_ + 1, $_ + ($_ + 1) }, ^Inf;
 
my @wanted = @n-n1-triangular.grep: *.[2].is-prime;
 
printf "%2d + %2d = %2d\n", |.list for @wanted.head(20);
```

#### Output:
```
 1 +  2 =  3
 2 +  3 =  5
 3 +  4 =  7
 5 +  6 = 11
 6 +  7 = 13
 8 +  9 = 17
 9 + 10 = 19
11 + 12 = 23
14 + 15 = 29
15 + 16 = 31
18 + 19 = 37
20 + 21 = 41
21 + 22 = 43
23 + 24 = 47
26 + 27 = 53
29 + 30 = 59
30 + 31 = 61
33 + 34 = 67
35 + 36 = 71
36 + 37 = 73
```
