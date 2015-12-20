[1]: http://rosettacode.org/wiki/Letter_frequency

# [Letter frequency][1]

In perl6, whenever you want to count things in a collection, the rule of thumb is to use the Bag structure.

```perl
say bag lines».comb
```