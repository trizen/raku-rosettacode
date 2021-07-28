[1]: https://rosettacode.org/wiki/Numbers_which_binary_and_ternary_digit_sum_are_prime

# [Numbers which binary and ternary digit sum are prime][1]

```perl
say (^200).grep(-> $n {all (2,3).map({$n.base($_).comb.sum.is-prime}) }).batch(10)».fmt('%3d').join: "\n";
```

#### Output:
```
  5   6   7  10  11  12  13  17  18  19
 21  25  28  31  33  35  36  37  41  47
 49  55  59  61  65  67  69  73  79  82
 84  87  91  93  97 103 107 109 115 117
121 127 129 131 133 137 143 145 151 155
157 162 167 171 173 179 181 185 191 193
199
```
