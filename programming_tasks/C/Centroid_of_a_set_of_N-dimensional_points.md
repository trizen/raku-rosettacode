[1]: https://rosettacode.org/wiki/Centroid_of_a_set_of_N-dimensional_points

# [Centroid of a set of N-dimensional points][1]

```perl
sub centroid { ( [»+«] @^LoL ) »/» +@^LoL }

say .&centroid for
    ( (1,), (2,), (3,) ),
    ( (8, 2), (0, 0) ),
    ( (5, 5, 0), (10, 10, 0) ),
    ( (1, 3.1, 6.5), (-2, -5, 3.4), (-7, -4, 9), (2, 0, 3) ),
    ( (0, 0, 0, 0, 1), (0, 0, 0, 1, 0), (0, 0, 1, 0, 0), (0, 1, 0, 0, 0) ),
;
```

#### Output:
```
(2)
(4 1)
(7.5 7.5 0)
(-1.5 -1.475 5.475)
(0 0.25 0.25 0.25 0.25)
```
