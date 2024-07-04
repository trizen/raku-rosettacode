[1]: https://rosettacode.org/wiki/Two_identical_strings

# [Two identical strings][1]

```perl
my @cat = (1..*).map: { :2([~] .base(2) xx 2) };
say "{+$_} matching numbers\n{.batch(5)».map({$_ ~ .base(2).fmt('(%s)')})».fmt('%15s').join: "\n"}\n"
    given @cat[^(@cat.first: * > 1000, :k)];
```

#### Output:
```
30 matching numbers
          3(11)        10(1010)        15(1111)      36(100100)      45(101101)
     54(110110)      63(111111)   136(10001000)   153(10011001)   170(10101010)
  187(10111011)   204(11001100)   221(11011101)   238(11101110)   255(11111111)
528(1000010000) 561(1000110001) 594(1001010010) 627(1001110011) 660(1010010100)
693(1010110101) 726(1011010110) 759(1011110111) 792(1100011000) 825(1100111001)
858(1101011010) 891(1101111011) 924(1110011100) 957(1110111101) 990(1111011110)
```