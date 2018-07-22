[1]: https://rosettacode.org/wiki/Jaro_distance

# [Jaro distance][1]

```perl
sub jaro ($s, $t) {
 
    return 1 if $s eq $t;
 
    my $s_len = + my @s = $s.comb;
    my $t_len = + my @t = $t.comb;
 
    my $match_distance = ($s_len max $t_len) div 2 - 1;
 
    my @s_matches;
    my @t_matches;
    my $matches = 0;
 
    for ^@s -> $i {
 
        my $start = 0 max $i - $match_distance;
        my $end = $i + $match_distance min $t_len;
 
        for $start .. $end -> $j {
            @t_matches[$j] and next;
            @s[$i] eq @t[$j] or next;
            @s_matches[$i] = 1;
            @t_matches[$j] = 1;
            $matches++;
            last;
        }
    }
 
    return 0 if $matches == 0;
 
    my $k              = 0;
    my $transpositions = 0;
 
    for ^@s -> $i {
        @s_matches[$i] or next;
        until @t_matches[$k] { ++$k }
        @s[$i] eq @t[$k] or ++$transpositions;
        ++$k;
    }
 
    ($matches / $s_len + $matches / $t_len +
        (($matches - $transpositions / 2) / $matches)) / 3;
}
 
printf("%f\n", jaro("MARTHA",    "MARHTA"));
printf("%f\n", jaro("DIXON",     "DICKSONX"));
printf("%f\n", jaro("JELLYFISH", "SMELLYFISH"));
```

#### Output:
```
0.944444
0.766667
0.896296
```