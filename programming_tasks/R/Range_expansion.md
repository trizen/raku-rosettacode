[1]: http://rosettacode.org/wiki/Range_expansion

# [Range expansion][1]

```perl
sub range-expansion (Str $range-description) {
    my $range-pattern = rx/ ( '-'? \d+ ) '-' ( '-'? \d+) /;
    my &expand = -> $term { $term ~~ $range-pattern ?? +$0..+$1 !! $term };
    return $range-description.split(',').map(&expand)
}
 
say range-expansion('-6,-3--1,3-5,7-11,14,15,17-20').join(', ');
```

#### Output:
```
-6, -3, -2, -1, 3, 4, 5, 7, 8, 9, 10, 11, 14, 15, 17, 18, 19, 20
```