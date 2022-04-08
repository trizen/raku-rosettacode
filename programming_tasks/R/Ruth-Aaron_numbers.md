[1]: https://rosettacode.org/wiki/Ruth-Aaron_numbers

# [Ruth-Aaron numbers][1]

```perl
use Prime::Factor;
 
my @pf  = lazy (^∞).hyper(:1000batch).map: *.&prime-factors.sum;
my @upf = lazy (^∞).hyper(:1000batch).map: *.&prime-factors.unique.sum;
 
# Task: < 1 second
put "First 30 Ruth-Aaron numbers (Factors):\n" ~
(1..∞).grep( { @pf[$_] == @pf[$_ + 1] } )[^30];
 
put "\nFirst 30 Ruth-Aaron numbers (Divisors):\n" ~
(1..∞).grep( { @upf[$_] == @upf[$_ + 1] } )[^30];
 
# Stretch: ~ 5 seconds
put "\nFirst Ruth-Aaron triple (Factors):\n" ~
(1..∞).first: { @pf[$_] == @pf[$_ + 1] == @pf[$_ + 2] }
 
# Really, really, _really_ slow. 186(!) minutes... but with no cheating or "leg up".
put "\nFirst Ruth-Aaron triple (Divisors):\n" ~
(1..∞).first: { @upf[$_] == @upf[$_ + 1] == @upf[$_ + 2] }
```

#### Output:
```
First 30 Ruth-Aaron numbers (Factors):
5 8 15 77 125 714 948 1330 1520 1862 2491 3248 4185 4191 5405 5560 5959 6867 8280 8463 10647 12351 14587 16932 17080 18490 20450 24895 26642 26649

First 30 Ruth-Aaron numbers (Divisors):
5 24 49 77 104 153 369 492 714 1682 2107 2299 2600 2783 5405 6556 6811 8855 9800 12726 13775 18655 21183 24024 24432 24880 25839 26642 35456 40081

First Ruth-Aaron triple (Factors):
417162

First Ruth-Aaron triple (Divisors):
89460294
```
