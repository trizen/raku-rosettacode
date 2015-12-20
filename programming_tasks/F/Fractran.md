[1]: http://rosettacode.org/wiki/Fractran

# [Fractran][1]

A Fractran program potentially returns an infinite list, and infinite lists are a common data structure in Perl 6. The limit is therefore enforced only by slicing the infinite list.

```perl
sub ft (\n) {
    first Int, map (* * n).narrow,
        |<17/91 78/85 19/51 23/38 29/33 77/29 95/23 77/19 1/17 11/13 13/11 15/14 15/2 55/1>, 0
} 
constant FT = 2, &ft ... 0;
say FT[^100];
```

#### Output:
```
(2 15 825 725 1925 2275 425 390 330 290 770 910 170 156 132 116 308 364 68 4 30 225 12375 10875 28875 25375 67375 79625 14875 13650 2550 2340 1980 1740 4620 4060 10780 12740 2380 2184 408 152 92 380 230 950 575 2375 9625 11375 2125 1950 1650 1450 3850 4550 850 780 660 580 1540 1820 340 312 264 232 616 728 136 8 60 450 3375 185625 163125 433125 380625 1010625 888125 2358125 2786875 520625 477750 89250 81900 15300 14040 11880 10440 27720 24360 64680 56840 150920 178360 33320 30576 5712 2128 1288)
```


**Extra credit:**



We can weed out all the powers of two into another infinite constant list based on the first list. In this case the sequence is limited only by our patience, and a ^C from the terminal. The <tt>.msb</tt> method finds the most significant bit of an integer, which conveniently is the base-2 log of the power-of-two in question.

```perl
constant FT = 2, &ft ... 0;
constant FT2 = FT.grep: { not $_ +& ($_ - 1) }
for 1..* -> $i {
    given FT2[$i] {
        say $i, "\t", .msb, "\t", $_;
    }
}
```

#### Output:
```
1       2       4
2       3       8
3       5       32
4       7       128
5       11      2048
6       13      8192
7       17      131072
8       19      524288
9       23      8388608
10      29      536870912
11      31      2147483648
12      37      137438953472
13      41      2199023255552
14      43      8796093022208
15      47      140737488355328
16      53      9007199254740992
17      59      576460752303423488
18      61      2305843009213693952
19      67      147573952589676412928
20      71      2361183241434822606848
^C
```