[1]: https://rosettacode.org/wiki/Levenshtein_distance

# [Levenshtein distance][1]





Implementation of the Wikipedia algorithm. Since column 0 and row 0 are used for base distances, the original algorithm would require us to compare "@s[$i-1] eq @t[$j-1]", and reference $m and $n separately. Prepending an unused value (undef) onto @s and @t makes their indices align with the $i,$j numbering of @d, and lets us use .end instead of $m,$n.

```perl
sub levenshtein-distance ( Str $s, Str $t --> Int ) {
    my @s = *, |$s.comb;
    my @t = *, |$t.comb;

    my @d;
    @d[$_;  0] = $_ for ^@s.end;
    @d[ 0; $_] = $_ for ^@t.end;

    for 1..@s.end X 1..@t.end -> ($i, $j) {
        @d[$i; $j] = @s[$i] eq @t[$j]
            ??   @d[$i-1; $j-1]    # No operation required when eq
            !! ( @d[$i-1; $j  ],   # Deletion
                 @d[$i  ; $j-1],   # Insertion
                 @d[$i-1; $j-1],   # Substitution
               ).min + 1;
    }

    @d[*-1][*-1];
}

for <kitten sitting>, <saturday sunday>, <rosettacode raisethysword> -> ($s, $t) {
    say "Levenshtein distance('$s', '$t') == ", levenshtein-distance($s, $t)
}
```

#### Output:
```
Levenshtein distance('kitten', 'sitting') == 3
Levenshtein distance('saturday', 'sunday') == 3
Levenshtein distance('rosettacode', 'raisethysword') == 8
```
