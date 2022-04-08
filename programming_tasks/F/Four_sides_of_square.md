[1]: https://rosettacode.org/wiki/Four_sides_of_square

# [Four sides of square][1]

This isn't a matrix, especially if it is supposed to be graphical; it's a very small (or extremely low resolution) bitmap.

```perl
sub hollow ($n) { [1 xx $n], |(0 ^..^ $n).map( { [flat 1, 0 xx $n - 2, 1] } ), [1 xx $n] }
 
.put for hollow 7;
put '';
.put for hollow 10;
```

#### Output:
```
1 1 1 1 1 1 1
1 0 0 0 0 0 1
1 0 0 0 0 0 1
1 0 0 0 0 0 1
1 0 0 0 0 0 1
1 0 0 0 0 0 1
1 0 0 0 0 0 1
1 1 1 1 1 1 1

1 1 1 1 1 1 1 1 1 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 0 0 0 0 0 0 0 0 1
1 1 1 1 1 1 1 1 1 1
```
