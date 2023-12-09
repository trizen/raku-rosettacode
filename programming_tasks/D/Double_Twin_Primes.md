[1]: https://rosettacode.org/wiki/Double_Twin_Primes

# [Double Twin Primes][1]

Cousin twin primes:

```perl
sub dt { $^p, $p+2, $p+6, $p+8 }
.&dt.say for (^1000).grep: { all .&dt».is-prime };
```

#### Output:
```
(5 7 11 13)
(11 13 17 19)
(101 103 107 109)
(191 193 197 199)
(821 823 827 829)
```
