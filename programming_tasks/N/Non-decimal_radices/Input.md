[1]: https://rosettacode.org/wiki/Non-decimal_radices/Input

# [Non-decimal radices/Input][1]


By default, all strings of digits are parsed as base 10 numbers, including those with a leading zero. Numbers with a prefix 0b, 0o, 0d or 0x are parsed as binary, octal, decimal or hexadecimal respectively.

```perl
say 0b11011;  # -> 27
say 0o11011;  # -> 4617
say 0d11011;  # -> 11011
say 0x11011;  # -> 69649
```


Additionally, there are built-in adverbial prefix operators to parse strings of "digits" of radix 2 through radix 36 into decimal. They will fail with a runtime error if they are fed a digit that is not valid in that radix.

```perl
my $n = '11011';

say  :2($n); # -> 27
say  :3($n); # -> 112
say  :4($n); # -> 325
say  :5($n); # -> 756
say  :6($n); # -> 1519
say  :7($n); # -> 2752
say  :8($n); # -> 4617
say  :9($n); # -> 7300
say :10($n); # -> 11011
say :11($n); # -> 15984
say :12($n); # -> 22477
say :13($n); # -> 30772
say :14($n); # -> 41175
say :15($n); # -> 54016
say :16($n); # -> 69649
say :17($n); # -> 88452
say :18($n); # -> 110827
say :19($n); # -> 137200
say :20($n); # -> 168021
say :21($n); # -> 203764
say :22($n); # -> 244927
say :23($n); # -> 292032
say :24($n); # -> 345625
say :25($n); # -> 406276
say :26($n); # -> 474579
say :27($n); # -> 551152
say :28($n); # -> 636637
say :29($n); # -> 731700
say :30($n); # -> 837031
say :31($n); # -> 953344
say :32($n); # -> 1081377
say :33($n); # -> 1221892
say :34($n); # -> 1375675
say :35($n); # -> 1543536
say :36($n); # -> 1726309
```
