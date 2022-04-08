[1]: https://rosettacode.org/wiki/Generate_random_numbers_without_repeating_a_value

# [Generate random numbers without repeating a value][1]

Raku has three distinct "random" functions built in. rand() for when you want some fraction between 0 and 1. roll() when you want to select elements from a collection with replacement (rolls of a die). And pick() for when you want to select some elements from a collection *without* replacement. (pick a card, any card, or two cards or 10 cards...). If you want to select *all* the elements in random order, just pick 'whatever'. Here we'll pick all from 1 to 20, 5 times using the repetition operator.

```perl
.put for (1..20).pick(*) xx 5
```

#### Output:
```
20 4 5 7 15 19 2 16 8 6 3 12 14 13 10 18 9 17 1 11
4 5 18 10 13 3 1 11 6 2 19 8 12 7 16 17 14 20 15 9
14 8 15 11 17 4 3 10 18 7 16 13 1 20 12 9 6 5 19 2
7 5 15 11 12 18 17 3 20 6 13 19 14 2 16 10 4 9 8 1
19 12 4 7 3 20 13 17 5 8 6 15 10 18 1 11 2 14 16 9
```
