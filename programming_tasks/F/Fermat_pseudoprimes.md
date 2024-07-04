[1]: https://rosettacode.org/wiki/Fermat_pseudoprimes

# [Fermat pseudoprimes][1]

```perl
use List::Divvy;
for 1..20 -> $base {
    my @pseudo = lazy (2..*).hyper.grep: { !.is-prime && (1 == expmod $base, $_ - 1, $_) }
    my $threshold = 100_000;
    say $base.fmt("Base %2d - Up to $threshold: ") ~ (+@pseudo.&upto: $threshold).fmt('%5d')
        ~ "  First 20: " ~ @pseudo[^20].gist
}
```

#### Output:
```
Base  1 - Up to 100000: 90407  First 20: (4 6 8 9 10 12 14 15 16 18 20 21 22 24 25 26 27 28 30 32)
Base  2 - Up to 100000:    78  First 20: (341 561 645 1105 1387 1729 1905 2047 2465 2701 2821 3277 4033 4369 4371 4681 5461 6601 7957 8321)
Base  3 - Up to 100000:    78  First 20: (91 121 286 671 703 949 1105 1541 1729 1891 2465 2665 2701 2821 3281 3367 3751 4961 5551 6601)
Base  4 - Up to 100000:   153  First 20: (15 85 91 341 435 451 561 645 703 1105 1247 1271 1387 1581 1695 1729 1891 1905 2047 2071)
Base  5 - Up to 100000:    73  First 20: (4 124 217 561 781 1541 1729 1891 2821 4123 5461 5611 5662 5731 6601 7449 7813 8029 8911 9881)
Base  6 - Up to 100000:   104  First 20: (35 185 217 301 481 1105 1111 1261 1333 1729 2465 2701 2821 3421 3565 3589 3913 4123 4495 5713)
Base  7 - Up to 100000:    73  First 20: (6 25 325 561 703 817 1105 1825 2101 2353 2465 3277 4525 4825 6697 8321 10225 10585 10621 11041)
Base  8 - Up to 100000:   218  First 20: (9 21 45 63 65 105 117 133 153 231 273 341 481 511 561 585 645 651 861 949)
Base  9 - Up to 100000:   164  First 20: (4 8 28 52 91 121 205 286 364 511 532 616 671 697 703 946 949 1036 1105 1288)
Base 10 - Up to 100000:    90  First 20: (9 33 91 99 259 451 481 561 657 703 909 1233 1729 2409 2821 2981 3333 3367 4141 4187)
Base 11 - Up to 100000:    89  First 20: (10 15 70 133 190 259 305 481 645 703 793 1105 1330 1729 2047 2257 2465 2821 4577 4921)
Base 12 - Up to 100000:   127  First 20: (65 91 133 143 145 247 377 385 703 1045 1099 1105 1649 1729 1885 1891 2041 2233 2465 2701)
Base 13 - Up to 100000:    91  First 20: (4 6 12 21 85 105 231 244 276 357 427 561 1099 1785 1891 2465 2806 3605 5028 5149)
Base 14 - Up to 100000:    96  First 20: (15 39 65 195 481 561 781 793 841 985 1105 1111 1541 1891 2257 2465 2561 2665 2743 3277)
Base 15 - Up to 100000:    70  First 20: (14 341 742 946 1477 1541 1687 1729 1891 1921 2821 3133 3277 4187 6541 6601 7471 8701 8911 9073)
Base 16 - Up to 100000:   200  First 20: (15 51 85 91 255 341 435 451 561 595 645 703 1105 1247 1261 1271 1285 1387 1581 1687)
Base 17 - Up to 100000:    83  First 20: (4 8 9 16 45 91 145 261 781 1111 1228 1305 1729 1885 2149 2821 3991 4005 4033 4187)
Base 18 - Up to 100000:   134  First 20: (25 49 65 85 133 221 323 325 343 425 451 637 931 1105 1225 1369 1387 1649 1729 1921)
Base 19 - Up to 100000:   121  First 20: (6 9 15 18 45 49 153 169 343 561 637 889 905 906 1035 1105 1629 1661 1849 1891)
Base 20 - Up to 100000:   101  First 20: (21 57 133 231 399 561 671 861 889 1281 1653 1729 1891 2059 2413 2501 2761 2821 2947 3059)
```