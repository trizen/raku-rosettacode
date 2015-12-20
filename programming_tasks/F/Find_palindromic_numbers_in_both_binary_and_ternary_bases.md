[1]: http://rosettacode.org/wiki/Find_palindromic_numbers_in_both_binary_and_ternary_bases

# [Find palindromic numbers in both binary and ternary bases][1]

Instead of searching for numbers that are palindromes in one base then checking the other, generate palindromic trinary numbers directly, then check to see if they are also binary palindromes (with additional simplifying constraints as noted in other entries). Outputs the list in decimal, binary and trinary. Getting the 6th number requires about 4 cpu minutes on an i7.

```perl
constant palindromes = 0, 1, gather for 1 .. * -> $p {
    my $pal = $p.base(3);
    my $n = :3($pal ~ '1' ~ $pal.flip);
    next if $n %% 2;
    my $b2 = $n.base(2);
    next if $b2.chars %% 2;
    next unless $b2 eq $b2.flip;
    take $n;
}
 
printf "%d, %s, %s\n", $_, .base(2), .base(3) for palindromes[^6];
```

#### Output:
```
0, 0, 0
1, 1, 1
6643, 1100111110011, 100010001
1422773, 101011011010110110101, 2200021200022
5415589, 10100101010001010100101, 101012010210101
90396755477, 1010100001100000100010000011000010101, 22122022220102222022122
```