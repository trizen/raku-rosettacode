[1]: https://rosettacode.org/wiki/Greatest_element_of_a_list

# [Greatest element of a list][1]


The built-in function works with any type that defines ordering.

```perl
say max 10, 4, 5, -2, 11;
say max <zero one two three four five six seven eight nine>;

# Even when the values and number of values aren't known until runtime
my @list = flat(0..9,'A'..'H').roll((^60).pick).rotor(4,:partial)».join.words;
say @list, ': ', max @list;
```

#### Output:
```
11
zero
[6808 013C 6D5B 4219 29G9 DC13 CA4F 55F3 AA06 0AGF DAB0 2]: DC13
```
