[1]: https://rosettacode.org/wiki/Kaprekar_numbers

# [Kaprekar numbers][1]

### String version (slow)

```perl
sub kaprekar( Int $n ) {
    my $sq = $n ** 2;
    for 0 ^..^ $sq.chars -> $i {
        my $x = +$sq.substr(0, $i);
        my $y = +$sq.substr($i) || return;
        return True if $x + $y == $n;
    }
    False;
}
 
print 1;
print " $_" if .&kaprekar for ^10000;
print "\n";
```

#### Output:
```
1 9 45 55 99 297 703 999 2223 2728 4879 4950 5050 5292 7272 7777 9999
```


### Numeric version (medium)

```perl
sub kaprekar( Int $n, Int :$base = 10 ) {
    my $hi = $n ** 2;
    my $lo = 0;
    loop (my $s = 1; $hi; $s *= $base) {
        $lo += ($hi % $base) * $s;
        $hi div= $base;
        return $hi,$lo if $lo + $hi == $n and $lo;
    }
    ();
}
 
print " $_" if .&kaprekar for ^10_000;
 
say "\n\nBase 10 Kaprekar numbers < :10<1_000_000> = ", [+] (1 if kaprekar $_ for ^1000000);
 
say "\nBase 17 Kaprekar numbers < :17<1_000_000>";
 
my &k17 = &kaprekar.assuming(:base(17));
 
for ^:17<1000000> -> $n {
    my ($h,$l) = k17 $n;
    next unless $l;
    my $n17 = $n.base(17);
    my $s17 = ($n * $n).base(17);
    my $h17 = $h.base(17);
    say "$n $n17 $s17 ($h17 + $s17.substr(* - $h17.chars))";
}
```

#### Output:
```
 1 9 45 55 99 297 703 999 2223 2728 4879 4950 5050 5292 7272 7777 9999

Base 10 Kaprekar numbers < :10<1_000_000> = 54

Base 17 Kaprekar numbers < :17<1_000_000>
1 1 1 (0 + 1)
16 G F1 (F + 1)
64 3D E2G (E + 2G)
225 D4 A52G (A5 + 2G)
288 GG GF01 (GF + 01)
1536 556 1B43B2 (1B4 + 3B2)
3377 BBB 8093B2 (809 + 3B2)
4912 GGG GGF001 (GGF + 001)
7425 18BD 24E166G (24E + 166G)
9280 1F1F 39B1B94 (39B + 1B94)
16705 36DB B992C42 (B99 + 2C42)
20736 43CD 10DE32FG (10DE + 32FG)
30016 61EB 23593F92 (2359 + 3F92)
36801 785D 351E433G (351E + 433G)
37440 7A96 37144382 (3714 + 4382)
46081 967B 52G94382 (52G9 + 4382)
46720 98B4 5575433G (5575 + 433G)
53505 AF26 6GA43F92 (6GA4 + 3F92)
62785 CD44 9A5532FG (9A55 + 32FG)
66816 DA36 AEG42C42 (AEG4 + 2C42)
74241 F1F2 D75F1B94 (D75F + 1B94)
76096 F854 E1F5166G (E1F5 + 166G)
83520 GGGG GGGF0001 (GGGF + 0001)
266224 33334 A2C52A07G (A2C5 + 2A07G)
1153633 DDDDD B3D5E2A07G (B3D5E + 2A07G)
1334529 FGACC F0540F1A78 (F054 + 0F1A78)
1419856 GGGGG GGGGF00001 (GGGGF + 00001)
1787968 146FCA 19G4C12DG7F (19G4C + 12DG7F)
3122497 236985 4E3BE1F95D8 (4E3BE + 1F95D8)
3773952 2B32B3 711CB2420F9 (711CB + 2420F9)
4243968 2GDE03 8FEGB27EG09 (8FEGB + 27EG09)
5108481 3A2D6F CG10B2E3C64 (CG10B + 2E3C64)
5561920 3FA16D F5EAE3043CG (F5EAE + 3043CG)
6031936 443CCD 110DDE332FFG (110DDE + 332FFG)
6896449 4E9C28 16A10C37GB1D (16A10C + 37GB1D)
7435233 54067B 1A72G93AA382 (1A72G9 + 3AA382)
8017920 5AGGB6 1EF1D43D1EF2 (1EF1D4 + 3D1EF2)
9223201 687534 2835C5403G7G (2835C5 + 403G7G)
9805888 6F6F6G 2DBE3F41C131 (2DBE3F + 41C131)
11140416 7E692A 3A997C43DGBF (3A997C + 43DGBF)
11209185 7F391E 3B58D543F059 (3B58D5 + 43F059)
12928384 91D7F3 4EF79B43F059 (4EF79B + 43F059)
12997153 92A7E7 4FD82943DGBF (4FD829 + 43DGBF)
14331681 A1A1A1 5GF07041C131 (5GF070 + 41C131)
14914368 A89BDD 685C5E403G7G (685C5E + 403G7G)
16119649 B6005B 79F2793D1EF2 (79F279 + 3D1EF2)
16702336 BCGA96 8267143AA382 (826714 + 3AA382)
17241120 C274E9 8B7ACD37GB1D (8B7ACD + 37GB1D)
18105633 CCD444 99A555332FFG (99A555 + 332FFG)
18575649 D16FA4 A12BE53043CG (A12BE5 + 3043CG)
19029088 D6E3A2 A9A83F2E3C64 (A9A83F + 2E3C64)
19893601 E032GE B953G527EG09 (B953G5 + 27EG09)
20363617 E5DE5E C1BD752420F9 (C1BD75 + 2420F9)
21015072 EDA78C CF11C41F95D8 (CF11C4 + 1F95D8)
22349601 FCA147 E9D1D912DG7F (E9D1D9 + 12DG7F)
22803040 G10645 F2FCDE0F1A78 (F2FCDE + 0F1A78)
24137568 GGGGGG GGGGGF000001 (GGGGGF + 000001)
```


Note that this algorithm allows the null string on the left, taken as zero, which automatically includes 1 as the first element of the sequence.



### Casting out nines (fast)

```perl
sub kaprekar-generator( :$base = 10 ) {
    my $base-m1 = $base - 1;
    gather loop (my $place = 1; ; ++$place) {
        my $nend = $base ** $place;
        loop (my $n = $base ** ($place - 1); $n < $nend; ++$n) {
            if $n * ($n - 1) %% $base-m1 {
                my $pend = $place * 2;
                loop (my $p = $place; $p < $pend; ++$p) {
                    my $B = $base ** $p;
                    my $lo = $n * ($B - $n) div ($B - 1);
                    my $hi = floor $n - $lo;
                    if $n * $n == $hi * $B + $lo and $lo {
                        take [$n, $hi, $lo];
                        last;
                    }
                }
            }
        }
    }
}
 
print " $_[0]" for kaprekar-generator() ...^ *.[0] >= 10_000;
say "\n";
 
say "Base 10 Kaprekar numbers < :10<1_000_000> = ", +(kaprekar-generator() ...^ *.[0] >= 1000000);
say '';
 
say "Base 17 Kaprekar numbers < :17<1_000_000>";
 
my &k17-gen = &kaprekar-generator.assuming(:base(17));
 
for k17-gen() ...^ *.[0] >= :17<1000000> -> @r {
    my ($n,$h,$l) = @r;
    my $n17 = $n.base(17);
    my $s = $n * $n;
    my $s17 = $s.base(17);
    my $h17 = $h.base(17);
    my $l17 = $l.base(17);
    $l17 = '0' x ($s17.chars - $h17.chars - $l17.chars) ~ $l17;
    say "$n $n17 $s17 ($h17 + $l17)";
}
```


(Same output.)