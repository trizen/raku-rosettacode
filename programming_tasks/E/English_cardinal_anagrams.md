[1]: https://rosettacode.org/wiki/English_cardinal_anagrams

# [English cardinal anagrams][1]

```perl
use Lingua::EN::Numbers;
use List::Allmax;

for 1_000, 10_000 {
    my %eca; # English cardinal anagrams
    (^$_).map: { %eca{.&cardinal.comb.sort.join}.push: $_ }

    once say "\nFirst 30 English cardinal anagrams:\n " ~
    (sort flat %eca.grep(+*.value > 1)».value.map: *.flat)[^30]\
    .batch(10)».fmt("%3d").join: "\n ";

    say "\nCount of English cardinal anagrams up to {.&comma}: " ~
        +%eca.grep(+*.value > 1)».value.map: *.flat;

    say "\nLargest group(s) of English cardinal anagrams up to {.&comma}:\n [" ~
        %eca.&all-max( :by(+*.value) )».value.sort.join("]\n [") ~ ']'
}
```

#### Output:
```
First 30 English cardinal anagrams:
  67  69  76  79  96  97 102 103 104 105
 106 107 108 109 112 122 123 124 125 126
 127 128 129 132 133 134 135 136 137 138

Count of English cardinal anagrams up to 1,000: 317

Largest group(s) of English cardinal anagrams up to 1,000:
 [679 697 769 796 967 976]

Count of English cardinal anagrams up to 10,000: 2534

Largest group(s) of English cardinal anagrams up to 10,000:
 [1679 1697 1769 1796 1967 1976 6179 6197 6791 6971 7169 7196 7691 7961 9167 9176 9671 9761]
 [2679 2697 2769 2796 2967 2976 6279 6297 6792 6972 7269 7296 7692 7962 9267 9276 9672 9762]
 [3679 3697 3769 3796 3967 3976 6379 6397 6793 6973 7369 7396 7693 7963 9367 9376 9673 9763]
 [4679 4697 4769 4796 4967 4976 6479 6497 6794 6974 7469 7496 7694 7964 9467 9476 9674 9764]
 [5679 5697 5769 5796 5967 5976 6579 6597 6795 6975 7569 7596 7695 7965 9567 9576 9675 9765]
 [6798 6879 6897 6978 7698 7869 7896 7968 8679 8697 8769 8796 8967 8976 9678 9768 9867 9876]
```
