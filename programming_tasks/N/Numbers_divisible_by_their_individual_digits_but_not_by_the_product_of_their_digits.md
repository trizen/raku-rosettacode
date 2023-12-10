[1]: https://rosettacode.org/wiki/Numbers_divisible_by_their_individual_digits,_but_not_by_the_product_of_their_digits.

# [Numbers divisible by their individual digits, but not by the product of their digits.][1]

```perl
say "{+$_} matching numbers:\n{.batch(10)».fmt('%3d').join: "\n"}" given
   (^1000).grep: -> $n { $n.contains(0) ?? False !! all |($n.comb).map($n %% *), $n % [*] $n.comb };
```

#### Output:
```
45 matching numbers:
 22  33  44  48  55  66  77  88  99 122
124 126 155 162 168 184 222 244 248 264
288 324 333 336 366 396 412 424 444 448
488 515 555 636 648 666 728 777 784 824
848 864 888 936 999
```
