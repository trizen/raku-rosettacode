[1]: http://rosettacode.org/wiki/Sum_digits_of_an_integer

# [Sum digits of an integer][1]

This will handle input numbers in any base from 2 to 36.
The results are in base 10.

```perl
say Σ $_ for <1 1234 1020304 fe f0e DEADBEEF>;
 
sub Σ { [+] $^n.comb.map: { :36($_) } }
```

#### Output:
```
1
10
10
29
29
104
```