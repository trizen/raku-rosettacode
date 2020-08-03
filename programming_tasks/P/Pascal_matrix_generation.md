[1]: https://rosettacode.org/wiki/Pascal_matrix_generation

# [Pascal matrix generation][1]

Here is a rather more general solution than required. The `grow-matrix` function will grow any N by N matrix into an N+1 x N+1 matrix, using any function of the three leftward/upward neighbors, here labelled "West", "North", and "Northwest". We then define three iterator functions that can grow Pascal matrices, and use those iterators to define three constants, each of which is an infinite sequence of ever-larger Pascal matrices. Normal subscripting then pulls out the ones of the specified size.

```perl
# Extend a matrix in 2 dimensions based on 3 neighbors.
sub grow-matrix(@matrix, &func) {
    my $n = @matrix.shape eq '*' ?? 1 !! @matrix.shape[0];
    my @m[$n+1;$n+1];
    for ^$n X ^$n -> ($i, $j) {
       @m[$i;$j] = @matrix[$i;$j];
    }
#                     West         North        NorthWest
    @m[$n; 0] = func( 0,           @m[$n-1;0],  0            );
    @m[ 0;$n] = func( @m[0;$n-1],  0,           0            );
    @m[$_;$n] = func( @m[$_;$n-1], @m[$_-1;$n], @m[$_-1;$n-1]) for 1 ..^ $n;
    @m[$n;$_] = func( @m[$n;$_-1], @m[$n-1;$_], @m[$n-1;$_-1]) for 1 ..  $n;
    @m;
}
 
# I am but mad north-northwest...
sub madd-n-nw(@m) { grow-matrix @m, -> $w, $n, $nw {  $n + $nw } }
sub madd-w-nw(@m) { grow-matrix @m, -> $w, $n, $nw {  $w + $nw } }
sub madd-w-n (@m) { grow-matrix @m, -> $w, $n, $nw {  $w + $n  } }
 
# Define 3 infinite sequences of Pascal matrices.
constant upper-tri = [1], &madd-w-nw ... *;
constant lower-tri = [1], &madd-n-nw ... *;
constant symmetric = [1], &madd-w-n  ... *;
 
show_m upper-tri[4];
show_m lower-tri[4];
show_m symmetric[4];
 
sub show_m (@m) {
my \n = @m.shape[0];
for ^n X ^n -> (\i, \j) {
    print @m[i;j].fmt("%{1+max(@m).chars}d"); 
    print "\n" if j+1 eq n;
}
say '';
}
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

  1  1  1  1  1
  1  2  3  4  5
  1  3  6 10 15
  1  4 10 20 35
  1  5 15 35 70
```