[1]: https://rosettacode.org/wiki/Cullen_and_Woodall_numbers

# [Cullen and Woodall numbers][1]

```perl
my @cullen  = ^∞ .map: { $_ × 1 +< $_ + 1 };
my @woodall = ^∞ .map: { $_ × 1 +< $_ - 1 };
 
put "First 20 Cullen numbers: ( n × 2**n + 1)\n",     @cullen[1..20]; # A002064
put "\nFirst 20 Woodall numbers: ( n × 2**n - 1)\n", @woodall[1..20]; # A003261
put "\nFirst 5 Cullen primes: (in terms of n)\n",     @cullen.grep( &is-prime, :k )[^5];  # A005849
put "\nFirst 12 Woodall primes:  (in terms of n)\n", @woodall.grep( &is-prime, :k )[^12]; # A002234
```

#### Output:
```
First 20 Cullen numbers: ( n × 2**n + 1)
3 9 25 65 161 385 897 2049 4609 10241 22529 49153 106497 229377 491521 1048577 2228225 4718593 9961473 20971521

First 20 Woodall numbers: ( n × 2**n - 1)
1 7 23 63 159 383 895 2047 4607 10239 22527 49151 106495 229375 491519 1048575 2228223 4718591 9961471 20971519

First 5 Cullen primes: (in terms of n)
1 141 4713 5795 6611

First 12 Woodall primes:  (in terms of n)
2 3 6 30 75 81 115 123 249 362 384 462
```
