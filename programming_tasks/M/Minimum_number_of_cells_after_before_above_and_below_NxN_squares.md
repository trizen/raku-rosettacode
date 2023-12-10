[1]: https://rosettacode.org/wiki/Minimum_number_of_cells_after,_before,_above_and_below_NxN_squares

# [Minimum number of cells after, before, above and below NxN squares][1]

```perl
sub distance-to-edge (\N) {
   my $c = ceiling N / 2;
   my $f = floor   N / 2;
   my @ul = ^$c .map: -> $x { [ ^$c .map: { min($x, $_) } ] }
   @ul[$_].append: reverse @ul[$_; ^$f] for ^$c;
   @ul.push: [ reverse @ul[$_] ] for reverse ^$f;
   @ul
}
 
for 0, 1, 2, 6, 9, 23 {
    my @dte = .&distance-to-edge;
    my $max = chars max flat @dte».Slip;
 
    say "\n$_ x $_ distance to nearest edge:";
    .fmt("%{$max}d").say for @dte;
}
```

#### Output:
```
0 x 0 distance to nearest edge:

1 x 1 distance to nearest edge:
0

2 x 2 distance to nearest edge:
0 0
0 0

6 x 6 distance to nearest edge:
0 0 0 0 0 0
0 1 1 1 1 0
0 1 2 2 1 0
0 1 2 2 1 0
0 1 1 1 1 0
0 0 0 0 0 0

9 x 9 distance to nearest edge:
0 0 0 0 0 0 0 0 0
0 1 1 1 1 1 1 1 0
0 1 2 2 2 2 2 1 0
0 1 2 3 3 3 2 1 0
0 1 2 3 4 3 2 1 0
0 1 2 3 3 3 2 1 0
0 1 2 2 2 2 2 1 0
0 1 1 1 1 1 1 1 0
0 0 0 0 0 0 0 0 0

23 x 23 distance to nearest edge:
 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
 0  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  0
 0  1  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  1  0
 0  1  2  3  3  3  3  3  3  3  3  3  3  3  3  3  3  3  3  3  2  1  0
 0  1  2  3  4  4  4  4  4  4  4  4  4  4  4  4  4  4  4  3  2  1  0
 0  1  2  3  4  5  5  5  5  5  5  5  5  5  5  5  5  5  4  3  2  1  0
 0  1  2  3  4  5  6  6  6  6  6  6  6  6  6  6  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  7  7  7  7  7  7  7  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  8  8  8  8  8  8  8  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  8  9  9  9  9  9  8  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  8  9 10 10 10  9  8  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  8  9 10 11 10  9  8  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  8  9 10 10 10  9  8  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  8  9  9  9  9  9  8  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  8  8  8  8  8  8  8  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  7  7  7  7  7  7  7  7  7  6  5  4  3  2  1  0
 0  1  2  3  4  5  6  6  6  6  6  6  6  6  6  6  6  5  4  3  2  1  0
 0  1  2  3  4  5  5  5  5  5  5  5  5  5  5  5  5  5  4  3  2  1  0
 0  1  2  3  4  4  4  4  4  4  4  4  4  4  4  4  4  4  4  3  2  1  0
 0  1  2  3  3  3  3  3  3  3  3  3  3  3  3  3  3  3  3  3  2  1  0
 0  1  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  2  1  0
 0  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  1  0
 0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  0
```
