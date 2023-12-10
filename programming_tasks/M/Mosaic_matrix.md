[1]: https://rosettacode.org/wiki/Mosaic_matrix

# [Mosaic matrix][1]

This isn't a matrix, especially if it is supposed to be graphical; it's a very small (or extremely low resolution) bitmap.

```perl
sub checker ($n) { (^$n).map: { 1 xx $n Z× (flat ($_ %% 2, $_ % 2) xx *) } }

.put for checker 7;
put '';
.put for checker 8;
```

#### Output:
```
1 0 1 0 1 0 1
0 1 0 1 0 1 0
1 0 1 0 1 0 1
0 1 0 1 0 1 0
1 0 1 0 1 0 1
0 1 0 1 0 1 0
1 0 1 0 1 0 1

1 0 1 0 1 0 1 0
0 1 0 1 0 1 0 1
1 0 1 0 1 0 1 0
0 1 0 1 0 1 0 1
1 0 1 0 1 0 1 0
0 1 0 1 0 1 0 1
1 0 1 0 1 0 1 0
0 1 0 1 0 1 0 1
```
