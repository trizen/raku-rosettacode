[1]: https://rosettacode.org/wiki/Minimum_positive_multiple_in_base_10_using_only_0_and_1

# [Minimum positive multiple in base 10 using only 0 and 1][1]





Naive, brute force. Simplest thing that could possibly work. Will find any B10 eventually (until you run out of memory or patience) but sloooow, especially for larger multiples of 9.

```perl
say $_ , ': ', (1..*).map( *.base(2) ).first: * %% $_ for flat 1..10, 95..105; # etc.
```


Based on [Ed Pegg jr.s C code](http://www.mathpuzzle.com/Binary.html) from Mathpuzzlers.com. Similar to Phix and Go entries.

```perl
sub Ed-Pegg-jr (\n) {
    return 1 if n == 1;
    my ($count, $power-mod-n) = 0, 1;
    my @oom-mod-n = 0; # orders of magnitude of 10 mod n
    my @dig-mod = 0;   # 1 to n + oom mod n
    for 1..n -> \i {
        @oom-mod-n[i] = $power-mod-n;
        for 1..n -> \j {
            my \k = (j + $power-mod-n - 1) % n + 1;
            @dig-mod[k] = i if @dig-mod[j] and @dig-mod[j] != i and !@dig-mod[k];
        }
        @dig-mod[$power-mod-n + 1] ||= i;
        ($power-mod-n *= 10) %= n;
        last if @dig-mod[1];
    }
    my ($b10, $remainder) = '', n;
    while $remainder {
        my $place = @dig-mod[$remainder % n + 1];
        $b10 ~= '0' x ($count - $place) if $count > $place;
        $count = $place - 1;
        $b10 ~= '1';
        $remainder = (n + $remainder - @oom-mod-n[$place]) % n;
    }
    $b10 ~ '0' x $count
}

printf "%5s: %28s  %s\n", 'Number', 'B10', 'Multiplier';

for flat 1..10, 95..105, 297, 576, 594, 891, 909, 999, 1998, 2079, 2251, 2277, 2439, 2997, 4878 {
    printf "%6d: %28s  %s\n", $_, my $a = Ed-Pegg-jr($_), $a / $_;
}
```

#### Output:
```
Number:                          B10  Multiplier
     1:                            1  1
     2:                           10  5
     3:                          111  37
     4:                          100  25
     5:                           10  2
     6:                         1110  185
     7:                         1001  143
     8:                         1000  125
     9:                    111111111  12345679
    10:                           10  1
    95:                       110010  1158
    96:                     11100000  115625
    97:                     11100001  114433
    98:                     11000010  112245
    99:           111111111111111111  1122334455667789
   100:                          100  1
   101:                          101  1
   102:                      1000110  9805
   103:                     11100001  107767
   104:                      1001000  9625
   105:                       101010  962
   297:          1111011111111111111  3740778151889263
   576:              111111111000000  192901234375
   594:         11110111111111111110  18703890759446315
   891:          1111111111111111011  1247038284075321
   909:          1011111111111111111  1112333455567779
   999:  111111111111111111111111111  111222333444555666777889
  1998: 1111111111111111111111111110  556111667222778333889445
  2079:       1001101101111111111111  481530111164555609
  2251:                 101101101111  44913861
  2277:         11110111111111111011  4879275850290343
  2439:   10000101011110111101111111  4100082415379299344449
  2997: 1111110111111111111111111111  370740777814851888925963
  4878:  100001010111101111011111110  20500412076896496722245
```
