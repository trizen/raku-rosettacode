[1]: https://rosettacode.org/wiki/Powerful_numbers

# [Powerful numbers][1]




Perl 6 has no handy pre-made nth integer root routine so has the same problem as Go with needing a slight "fudge factor" in the nth root calculation.

```perl
sub super (\x) { x.trans([<0123456789>.comb] => [<⁰¹²³⁴⁵⁶⁷⁸⁹>.comb]) }
 
sub is-square-free (Int \n) {
    constant @p = ^100 .map: { next unless .is-prime; .² };
    for @p -> \p { return False if n %% p }
    True
}
 
sub powerfuls (\n, \k, \enumerate = False) {
    my @powerful;
    p(1, 2*k - 1);
    sub p (\m, \r) {
        if r < k {
            enumerate ?? @powerful.push(m) !! ++@powerful[m - 1  ?? (m - 1).chars !! 0];
            return
        }
        for 1 .. ((n / m) ** (1/r) + .0001).Int -> \v {
            if r > k {
                next unless is-square-free(v);
                next unless m gcd v == 1;
            }
            p(m * v ** r, r - 1)
        }
    }
    @powerful;
}
 
put "Count and first and last five enumerated n-powerful numbers in 10ⁿ:";
for 2..10 -> \k {
    my @powerful = sort powerfuls(10**k, k, True);
    printf "%2d %2s-powerful numbers <= 10%-2s: %s ... %s\n", +@powerful, k, super(k),
           @powerful.head(5).join(", "), @powerful.tail(5).join(", ");
}
 
put "\nCounts in each order of magnitude:";
my $top = 9;
for 2..10 -> \k {
    printf "%2s-powerful numbers <= 10ⁿ (where 0 <= n <= %d): ", k, $top+k;
    quietly say join ', ', [\+] powerfuls(10**($top + k), k);
}
```

#### Output:
```
Count and first and last five enumerated n-powerful numbers in 10ⁿ:
14  2-powerful numbers <= 10² : 1, 4, 8, 9, 16 ... 49, 64, 72, 81, 100
20  3-powerful numbers <= 10³ : 1, 8, 16, 27, 32 ... 625, 648, 729, 864, 1000
25  4-powerful numbers <= 10⁴ : 1, 16, 32, 64, 81 ... 5184, 6561, 7776, 8192, 10000
32  5-powerful numbers <= 10⁵ : 1, 32, 64, 128, 243 ... 65536, 69984, 78125, 93312, 100000
38  6-powerful numbers <= 10⁶ : 1, 64, 128, 256, 512 ... 559872, 746496, 823543, 839808, 1000000
46  7-powerful numbers <= 10⁷ : 1, 128, 256, 512, 1024 ... 7558272, 8388608, 8957952, 9765625, 10000000
52  8-powerful numbers <= 10⁸ : 1, 256, 512, 1024, 2048 ... 60466176, 67108864, 80621568, 90699264, 100000000
59  9-powerful numbers <= 10⁹ : 1, 512, 1024, 2048, 4096 ... 644972544, 725594112, 816293376, 967458816, 1000000000
68 10-powerful numbers <= 10¹⁰: 1, 1024, 2048, 4096, 8192 ... 7739670528, 8589934592, 8707129344, 9795520512, 10000000000

Counts in each order of magnitude:
 2-powerful numbers <= 10ⁿ (where 0 <= n <= 11): 1, 4, 14, 54, 185, 619, 2027, 6553, 21044, 67231, 214122, 680330
 3-powerful numbers <= 10ⁿ (where 0 <= n <= 12): 1, 2, 7, 20, 51, 129, 307, 713, 1645, 3721, 8348, 18589, 41136
 4-powerful numbers <= 10ⁿ (where 0 <= n <= 13): 1, 1, 5, 11, 25, 57, 117, 235, 464, 906, 1741, 3312, 6236, 11654
 5-powerful numbers <= 10ⁿ (where 0 <= n <= 14): 1, 1, 3, 8, 16, 32, 63, 117, 211, 375, 659, 1153, 2000, 3402, 5770
 6-powerful numbers <= 10ⁿ (where 0 <= n <= 15): 1, 1, 2, 6, 12, 21, 38, 70, 121, 206, 335, 551, 900, 1451, 2326, 3706
 7-powerful numbers <= 10ⁿ (where 0 <= n <= 16): 1, 1, 1, 4, 10, 16, 26, 46, 77, 129, 204, 318, 495, 761, 1172, 1799, 2740
 8-powerful numbers <= 10ⁿ (where 0 <= n <= 17): 1, 1, 1, 3, 8, 13, 19, 32, 52, 85, 135, 211, 315, 467, 689, 1016, 1496, 2191
 9-powerful numbers <= 10ⁿ (where 0 <= n <= 18): 1, 1, 1, 2, 6, 11, 16, 24, 38, 59, 94, 145, 217, 317, 453, 644, 919, 1308, 1868
10-powerful numbers <= 10ⁿ (where 0 <= n <= 19): 1, 1, 1, 1, 5, 9, 14, 21, 28, 43, 68, 104, 155, 227, 322, 447, 621, 858, 1192, 1651
```
