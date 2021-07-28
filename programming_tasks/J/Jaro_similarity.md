[1]: https://rosettacode.org/wiki/Jaro_similarity

# [Jaro similarity][1]



```perl
sub jaro ($s, $t) {
    return 1 if $s eq $t;
 
    my $s_len = + my @s = $s.comb;
    my $t_len = + my @t = $t.comb;
    my $match_distance = ($s_len max $t_len) div 2 - 1;
 
    my ($matches, @s_matches, @t_matches);
    for ^@s -> $i {
        my $start = 0 max $i - $match_distance;
        my $end   = $i + $match_distance min ($t_len - 1);
 
        for $start .. $end -> $j {
            next if @t_matches[$j] or @s[$i] ne @t[$j];
            (@s_matches[$i], @t_matches[$j]) = (1, 1);
            $matches++ and last;
        }
    }
    return 0 unless $matches;
 
    my ($k, $transpositions) = (0, 0);
    for ^@s -> $i {
        next unless @s_matches[$i];
        $k++ until  @t_matches[$k];
        $transpositions++ if @s[$i] ne @t[$k];
        $k++;
    }
 
    ( $matches/$s_len + $matches/$t_len + (($matches - $transpositions/2) / $matches) ) / 3
}
 
say jaro(.key, .value).fmt: '%.3f' for
    'MARTHA' => 'MARHTA', 'DIXON' => 'DICKSONX', 'JELLYFISH' => 'SMELLYFISH',
    'I repeat myself' => 'I repeat myself', '' => '';
```

#### Output:
```
0.944
0.767
0.896
1.000
1.000
```
