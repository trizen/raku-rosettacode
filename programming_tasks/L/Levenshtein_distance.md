[1]: https://rosettacode.org/wiki/Levenshtein_distance

# [Levenshtein distance][1]

Implementation of the wikipedia algorithm. Since column 0 and row 0 are used for base distances, the original algorithm would require us to compare "@s[$i-1] eq @t[$j-1]", and reference the $m and $n separately. Prepending an unused value (undef) onto @s and @t makes their indices align with the $i,$j numbering of @d, and lets us use .end instead of $m,$n.

```perl
sub levenshtein_distance ( Str $s, Str $t --> Int ) {
    my @s = *, |$s.comb;
    my @t = *, |$t.comb;
 
    my @d;
    @d[$_;  0] = $_ for ^@s.end;
    @d[ 0; $_] = $_ for ^@t.end;
 
    for 1..@s.end X 1..@t.end -> ($i, $j) {
        @d[$i; $j] = @s[$i] eq @t[$j]
            ??   @d[$i-1; $j-1]    # No operation required when eq
            !! ( @d[$i-1; $j  ],   # Deletion
                 @d[$i  ; $j-1],   # Insertion
                 @d[$i-1; $j-1],   # Substitution
               ).min + 1;
    }
 
    return @d[*-1][*-1];
}
 
my @a = [<kitten sitting>], [<saturday sunday>], [<rosettacode raisethysword>];
 
for @a -> [$s, $t] {
    say "levenshtein_distance('$s', '$t') == ", levenshtein_distance($s, $t);
}
```

#### Output:
```
levenshtein_distance('kitten', 'sitting') == 3
levenshtein_distance('saturday', 'sunday') == 3
levenshtein_distance('rosettacode', 'raisethysword') == 8
```