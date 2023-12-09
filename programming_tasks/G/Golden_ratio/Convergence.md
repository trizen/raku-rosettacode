[1]: https://rosettacode.org/wiki/Golden_ratio/Convergence

# [Golden ratio/Convergence][1]

```perl
constant phi = 1.FatRat, 1 + 1/* ... { abs($^a-$^b)≤1e-5 };

say "It took {phi.elems} iterations to reach {phi.tail}";
say "The error is {{ ($^a - $^b)/$^b }(phi.tail, (1+sqrt(5))/2)}"
```

#### Output:
```
It took 15 iterations to reach 1.618033
The error is -7.427932029059675e-07
```
