[1]: https://rosettacode.org/wiki/Extreme_primes

# [Extreme primes][1]

So we're just gonna keep doing the same task over and over with slightly different names?



Primes equal to the sum of the first k primes for some k.

```perl
use Lingua::EN::Numbers;

say $_».&comma».fmt("%7s").batch(10).join: "\n" for
(([\+] (^∞).grep: &is-prime).grep: &is-prime)[^30,999,1999,2999,3999,4999];
```

#### Output:
```
      2       5      17      41     197     281   7,699   8,893  22,039  24,133
 25,237  28,697  32,353  37,561  38,921  43,201  44,683  55,837  61,027  66,463
 70,241  86,453 102,001 109,147 116,533 119,069 121,631 129,419 132,059 263,171
1,657,620,079
9,744,982,591
24,984,473,177
49,394,034,691
82,195,983,953
```
