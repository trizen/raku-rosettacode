[1]: https://rosettacode.org/wiki/Super-d_numbers

# [Super-d numbers][1]





2 - 6 take a few seconds, 7 about 17 seconds, 8 about 90... 9, bleh... around 700 seconds.

```perl
sub super (\d) {
    my \run = d x d;
    ^∞ .hyper.grep: -> \n { (d * n ** d).Str.contains: run }
}

(2..9).race(:1batch).map: {
    my $now = now;
    put "\nFirst 10 super-$_ numbers:\n{.&super[^10]}\n{(now - $now).round(.1)} sec."
}
```

#### Output:
```
First 10 super-2 numbers:
19 31 69 81 105 106 107 119 127 131
0.1 sec.

First 10 super-3 numbers:
261 462 471 481 558 753 1036 1046 1471 1645
0.1 sec.

First 10 super-4 numbers:
1168 4972 7423 7752 8431 10267 11317 11487 11549 11680
0.3 sec.

First 10 super-5 numbers:
4602 5517 7539 12955 14555 20137 20379 26629 32767 35689
0.6 sec.

First 10 super-6 numbers:
27257 272570 302693 323576 364509 502785 513675 537771 676657 678146
5.2 sec.

First 10 super-7 numbers:
140997 490996 1184321 1259609 1409970 1783166 1886654 1977538 2457756 2714763
17.1 sec.

First 10 super-8 numbers:
185423 641519 1551728 1854230 6415190 12043464 12147605 15517280 16561735 18542300
92.1 sec.

First 10 super-9 numbers:
17546133 32613656 93568867 107225764 109255734 113315082 121251742 175461330 180917907 182557181
704.7 sec.
```
