[1]: https://rosettacode.org/wiki/Numbers_in_base_10_that_are_palindromic_in_bases_2,_4,_and_16

# [Numbers in base 10 that are palindromic in bases 2, 4, and 16][1]

```perl
put "{+$_} such numbers:\n", .batch(10)».fmt('%5d').join("\n") given
(^25000).grep: -> $n { all (2,4,16).map: { $n.base($_) eq $n.base($_).flip } }
```

#### Output:
```
23 such numbers:
    0     1     3     5    15    17    51    85   255   257
  273   771   819  1285  1365  3855  4095  4097  4369 12291
13107 20485 21845
```
