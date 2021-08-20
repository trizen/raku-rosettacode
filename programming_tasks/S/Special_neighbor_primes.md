[1]: https://rosettacode.org/wiki/Special_neighbor_primes

# [Special neighbor primes][1]

```perl
# 20210809 Raku programming solution 
 
for (grep {.is-prime}, 3..*).rotor(2 => -1) -> (\P1,\P2) {
   last if P2 ≥ Ⅽ;
   ($_ = P1+P2-1).is-prime and printf "%2d, %2d => %3d\n", P1, P2, $_
}
```

#### Output:
```
 3,  5 =>   7
 5,  7 =>  11
 7, 11 =>  17
11, 13 =>  23
13, 17 =>  29
19, 23 =>  41
29, 31 =>  59
31, 37 =>  67
41, 43 =>  83
43, 47 =>  89
61, 67 => 127
67, 71 => 137
73, 79 => 151
```
