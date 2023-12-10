[1]: https://rosettacode.org/wiki/Fractran

# [Fractran][1]





A Fractran program potentially returns an infinite list, and infinite lists are a common data structure in Raku.  The limit is therefore enforced only by slicing the infinite list.

```perl
sub fractran(@program) {
    2, { first Int, map (* * $_).narrow, @program } ... 0
}
say fractran(<17/91 78/85 19/51 23/38 29/33 77/29 95/23 77/19 1/17 11/13 13/11
        15/14 15/2 55/1>)[^100];
```

#### Output:
```
(2 15 825 725 1925 2275 425 390 330 290 770 910 170 156 132 116 308 364 68 4 30 225 12375 10875 28875 25375 67375 79625 14875 13650 2550 2340 1980 1740 4620 4060 10780 12740 2380 2184 408 152 92 380 230 950 575 2375 9625 11375 2125 1950 1650 1450 3850 4550 850 780 660 580 1540 1820 340 312 264 232 616 728 136 8 60 450 3375 185625 163125 433125 380625 1010625 888125 2358125 2786875 520625 477750 89250 81900 15300 14040 11880 10440 27720 24360 64680 56840 150920 178360 33320 30576 5712 2128 1288)
```


**Extra credit:**
We can weed out all the powers of two into another infinite constant list based on the first list.  In this case the sequence is limited only by our patience, and a ^C from the terminal.  The `.msb` method finds the most significant bit of an integer, which conveniently is the base-2 log of the power-of-two in question.

```perl
sub fractran(@program) {
    2, { first Int, map (* * $_).narrow, @program } ... 0
}
for fractran <17/91 78/85 19/51 23/38 29/33 77/29 95/23 77/19 1/17 11/13 13/11
        15/14 15/2 55/1> {
        say $++, "\t", .msb, "\t", $_ if 1 +< .msb == $_;
}
```

#### Output:
```
0       1       2
1       2       4
2       3       8
3       5       32
4       7       128
5       11      2048
6       13      8192
7       17      131072
8       19      524288
9       23      8388608
^C
```
