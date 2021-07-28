[1]: https://rosettacode.org/wiki/Sum_of_the_digits_of_n_is_substring_of_n

# [Sum of the digits of n is substring of n][1]

```perl
say "{+$_} matching numbers\n{.batch(10)».fmt('%3d').join: "\n"}" given (^1000).grep: { .contains: .comb.sum }
```

#### Output:
```
48 matching numbers
  0   1   2   3   4   5   6   7   8   9
 10  20  30  40  50  60  70  80  90 100
109 119 129 139 149 159 169 179 189 199
200 300 400 500 600 700 800 900 910 911
912 913 914 915 916 917 918 919
```
