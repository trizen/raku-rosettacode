[1]: http://rosettacode.org/wiki/Flatten_a_list

# [Flatten a list][1]

```perl
my @l = [[1], 2, [[3,4], 5], [[[]]], [[[6]]], 7, 8, []];
 
say .perl given gather @l.deepmap(*.take); # lazy recursive version
```