[1]: http://rosettacode.org/wiki/Remove_duplicate_elements

# [Remove duplicate elements][1]

```perl6
my @unique = [1, 2, 3, 5, 2, 4, 3, -3, 7, 5, 6].unique;
```


Or just make a set of it.

```perl6
set(1,2,3,5,2,4,3,-3,7,5,6).list
```