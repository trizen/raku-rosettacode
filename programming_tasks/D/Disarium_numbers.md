[1]: https://rosettacode.org/wiki/Disarium_numbers

# [Disarium numbers][1]

Not an efficient algorithm. First 18 in less than 1/4 second. 19th in around 45 seconds. Pretty much unusable for the 20th.

```perl
my $disarium = (^∞).hyper.map: { $_ if $_ == sum .polymod(10 xx *).reverse Z** 1..* };

put $disarium[^18];
put $disarium[18];
```

#### Output:
```
0 1 2 3 4 5 6 7 8 9 89 135 175 518 598 1306 1676 2427
2646798
```
