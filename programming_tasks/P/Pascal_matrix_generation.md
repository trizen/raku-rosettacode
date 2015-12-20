[1]: http://rosettacode.org/wiki/Pascal_matrix_generation

# [Pascal matrix generation][1]

Here is a rather more general solution than required. The <tt>grow-matrix</tt> function will grow any N by N matrix into an N+1 x N+1 matrix, using any function of the three leftward/upward neighbors, here labelled "West", "North", and "Northwest". We then define three iterator functions that can grow Pascal matrices, and use those iterators to define three constants, each of which is an infinite sequence of ever-larger Pascal matrices. Normal subscripting then pulls out the ones of the specified size.

```perl
# Extend a matrix in 2 dimensions based on 3 neighbors.
sub grow-matrix(@matrix, &func) {
    my @m = @matrix.deepmap: { .clone }
    my $s = +@m; #     West          North         NorthWest
    @m[$s][0]  = func( 0,            @m[$s-1][0],  0             );
    @m[0][$s]  = func( @m[0][$s-1],  0,            0             );
    @m[$_][$s] = func( @m[$_][$s-1], @m[$_-1][$s], @m[$_-1][$s-1]) for 1 ..^ $s;
    @m[$s][$_] = func( @m[$s][$_-1], @m[$s-1][$_], @m[$s-1][$_-1]) for 1 .. $s;
    [@m];
}
 
# I am but mad north-northwest...
sub madd-n-nw($m) { grow-matrix $m, -> $w, $n, $nw { $n + $nw } }
sub madd-w-nw($m) { grow-matrix $m, -> $w, $n, $nw { $w + $nw } }
sub madd-w-n ($m) { grow-matrix $m, -> $w, $n, $nw { $w + $n  } }
 
# Define 3 infinite sequences of Pascal matrices.
constant upper-tri := [[1]], &madd-w-nw ... *;
constant lower-tri := [[1]], &madd-n-nw ... *;
constant symmetric := [[1]], &madd-w-n  ... *;
 
# Pull out the 4th element of each sequence.
.say for upper-tri[4][]; say '';
.say for lower-tri[4][]; say '';
.say for symmetric[4][];
```

#### Output:
```
1 1 1 1 1
0 1 2 3 4
0 0 1 3 6
0 0 0 1 4
0 0 0 0 1

1 0 0 0 0
1 1 0 0 0
1 2 1 0 0
1 3 3 1 0
1 4 6 4 1

1 1 1 1 1
1 2 3 4 5
1 3 6 10 15
1 4 10 20 35
1 5 15 35 70
```