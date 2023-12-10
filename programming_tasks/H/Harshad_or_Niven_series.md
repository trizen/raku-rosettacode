[1]: https://rosettacode.org/wiki/Harshad_or_Niven_series

# [Harshad or Niven series][1]



```perl
constant @harshad = grep { $_ %% .comb.sum }, 1 .. *;
 
say @harshad[^20];
say @harshad.first: * > 1000;
```

#### Output:
```
(1 2 3 4 5 6 7 8 9 10 12 18 20 21 24 27 30 36 40 42)
1002
```
