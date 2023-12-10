[1]: https://rosettacode.org/wiki/Reduced_row_echelon_form

# [Reduced row echelon form][1]





### Following pseudocode

```perl
sub rref (@m) {
    my ($lead, $rows, $cols) = 0, @m, @m[0];
    for ^$rows -> $r {
        return @m unless $lead < $cols;
        my $i = $r;
        until @m[$i;$lead] {
            next unless ++$i == $rows;
            $i = $r;
            return @m if ++$lead == $cols;
        }
        @m[$i, $r] = @m[$r, $i] if $r != $i;
        @m[$r] »/=» $ = @m[$r;$lead];

        for ^$rows -> $n {
            next if $n == $r;
            @m[$n] »-=» @m[$r] »×» (@m[$n;$lead] // 0);
        }
        ++$lead;
    }
    @m
}

sub rat-or-int ($num) {
    return $num unless $num ~~ Rat;
    return $num.narrow if $num.narrow ~~ Int;
    $num.nude.join: '/';
}

sub say_it ($message, @array) {
    say "\n$message";
    $_».&rat-or-int.fmt(" %5s").say for @array;
}

my @M = (
    [ # base test case
      [  1,  2,  -1,  -4 ],
      [  2,  3,  -1, -11 ],
      [ -2,  0,  -3,  22 ],
    ],
    [ # mix of number styles
      [  3,   0,  -3,    1 ],
      [ .5, 3/2,  -3,   -2 ],
      [ .2, 4/5,  -1.6, .3 ],
    ],
    [ # degenerate case
      [ 1,  2,  3,  4,  3,  1],
      [ 2,  4,  6,  2,  6,  2],
      [ 3,  6, 18,  9,  9, -6],
      [ 4,  8, 12, 10, 12,  4],
      [ 5, 10, 24, 11, 15, -4],
    ],
    [ # larger matrix
      [1,  0,  0,  0,  0,  0,  1,  0,  0,  0,  0, -1,  0,  0,  0,  0,  0,  0],
      [1,  0,  0,  0,  0,  0,  0,  1,  0,  0,  0,  0, -1,  0,  0,  0,  0,  0],
      [1,  0,  0,  0,  0,  0,  0,  0,  1,  0,  0,  0,  0, -1,  0,  0,  0,  0],
      [0,  1,  0,  0,  0,  0,  1,  0,  0,  0,  0,  0,  0,  0, -1,  0,  0,  0],
      [0,  1,  0,  0,  0,  0,  0,  0,  1,  0,  0, -1,  0,  0,  0,  0,  0,  0],
      [0,  1,  0,  0,  0,  0,  0,  0,  0,  0,  1,  0,  0,  0,  0,  0, -1,  0],
      [0,  0,  1,  0,  0,  0,  1,  0,  0,  0,  0,  0, -1,  0,  0,  0,  0,  0],
      [0,  0,  1,  0,  0,  0,  0,  0,  0,  1,  0,  0,  0,  0, -1,  0,  0,  0],
      [0,  0,  0,  1,  0,  0,  0,  1,  0,  0,  0,  0,  0,  0,  0, -1,  0,  0],
      [0,  0,  0,  1,  0,  0,  0,  0,  0,  1,  0,  0, -1,  0,  0,  0,  0,  0],
      [0,  0,  0,  0,  1,  0,  0,  1,  0,  0,  0,  0,  0, -1,  0,  0,  0,  0],
      [0,  0,  0,  0,  1,  0,  0,  0,  1,  0,  0,  0,  0,  0,  0,  0, -1,  0],
      [0,  0,  0,  0,  1,  0,  0,  0,  0,  0,  1,  0,  0,  0,  0, -1,  0,  0],
      [0,  0,  0,  0,  0,  1,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0],
      [0,  0,  0,  0,  0,  0,  0,  0,  0,  1,  0,  0,  0,  0,  0,  0,  0,  0],
      [0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,  1,  0,  1],
      [0,  0,  0,  0,  0,  1,  0,  0,  0,  0,  1,  0,  0,  0, -1,  0,  0,  0],
   ]
);

for @M -> @matrix {
    say_it( 'Original Matrix', @matrix );
    say_it( 'Reduced Row Echelon Form Matrix', rref(@matrix) );
    say "\n";
}
```


Raku handles rational numbers internally as a ratio of two integers 
to maintain precision. 
For some situations it is useful to return the ratio 
rather than the floating point result.


```
Original Matrix
     1      2     -1     -4
     2      3     -1    -11
    -2      0     -3     22

Reduced Row Echelon Form Matrix
     1      0      0     -8
     0      1      0      1
     0      0      1     -2

Original Matrix
     3      0     -3      1
   1/2    3/2     -3     -2
   1/5    4/5   -8/5   3/10

Reduced Row Echelon Form Matrix
     1      0      0  -41/2
     0      1      0  -217/6
     0      0      1  -125/6

Original Matrix
     1      2      3      4      3      1
     2      4      6      2      6      2
     3      6     18      9      9     -6
     4      8     12     10     12      4
     5     10     24     11     15     -4

Reduced Row Echelon Form Matrix
     1      2      0      0      3      4
     0      0      1      0      0     -1
     0      0      0      1      0      0
     0      0      0      0      0      0
     0      0      0      0      0      0

Original Matrix
     1      0      0      0      0      0      1      0      0      0      0     -1      0      0      0      0      0      0
     1      0      0      0      0      0      0      1      0      0      0      0     -1      0      0      0      0      0
     1      0      0      0      0      0      0      0      1      0      0      0      0     -1      0      0      0      0
     0      1      0      0      0      0      1      0      0      0      0      0      0      0     -1      0      0      0
     0      1      0      0      0      0      0      0      1      0      0     -1      0      0      0      0      0      0
     0      1      0      0      0      0      0      0      0      0      1      0      0      0      0      0     -1      0
     0      0      1      0      0      0      1      0      0      0      0      0     -1      0      0      0      0      0
     0      0      1      0      0      0      0      0      0      1      0      0      0      0     -1      0      0      0
     0      0      0      1      0      0      0      1      0      0      0      0      0      0      0     -1      0      0
     0      0      0      1      0      0      0      0      0      1      0      0     -1      0      0      0      0      0
     0      0      0      0      1      0      0      1      0      0      0      0      0     -1      0      0      0      0
     0      0      0      0      1      0      0      0      1      0      0      0      0      0      0      0     -1      0
     0      0      0      0      1      0      0      0      0      0      1      0      0      0      0     -1      0      0
     0      0      0      0      0      1      0      0      0      0      0      0      0      0      0      0      0      0
     0      0      0      0      0      0      0      0      0      1      0      0      0      0      0      0      0      0
     0      0      0      0      0      0      0      0      0      0      0      0      0      0      0      1      0      1
     0      0      0      0      0      1      0      0      0      0      1      0      0      0     -1      0      0      0

Reduced Row Echelon Form Matrix
     1      0      0      0      0      0      0      0      0      0      0      0      0      0      0      0      0  17/39
     0      1      0      0      0      0      0      0      0      0      0      0      0      0      0      0      0   4/13
     0      0      1      0      0      0      0      0      0      0      0      0      0      0      0      0      0  20/39
     0      0      0      1      0      0      0      0      0      0      0      0      0      0      0      0      0  28/39
     0      0      0      0      1      0      0      0      0      0      0      0      0      0      0      0      0  19/39
     0      0      0      0      0      1      0      0      0      0      0      0      0      0      0      0      0      0
     0      0      0      0      0      0      1      0      0      0      0      0      0      0      0      0      0   8/39
     0      0      0      0      0      0      0      1      0      0      0      0      0      0      0      0      0  11/39
     0      0      0      0      0      0      0      0      1      0      0      0      0      0      0      0      0    1/3
     0      0      0      0      0      0      0      0      0      1      0      0      0      0      0      0      0      0
     0      0      0      0      0      0      0      0      0      0      1      0      0      0      0      0      0  20/39
     0      0      0      0      0      0      0      0      0      0      0      1      0      0      0      0      0  25/39
     0      0      0      0      0      0      0      0      0      0      0      0      1      0      0      0      0  28/39
     0      0      0      0      0      0      0      0      0      0      0      0      0      1      0      0      0  10/13
     0      0      0      0      0      0      0      0      0      0      0      0      0      0      1      0      0  20/39
     0      0      0      0      0      0      0      0      0      0      0      0      0      0      0      1      0      1
     0      0      0      0      0      0      0      0      0      0      0      0      0      0      0      0      1  32/39
```


### Row operations, procedural code



Re-implemented as elementary matrix row operations. Follow links for background on
[row operations](http://unapologetic.wordpress.com/2009/08/27/elementary-row-and-column-operations/)
and
[reduced row echelon form](http://unapologetic.wordpress.com/2009/09/03/reduced-row-echelon-form/)

```perl
sub    scale-row ( @M, \scale, \r       ) { @M[r]  =              @M[r]  »×» scale   }
sub    shear-row ( @M, \scale, \r1, \r2 ) { @M[r1] = @M[r1] »+» ( @M[r2] »×» scale ) }
sub   reduce-row ( @M,         \r,  \c  ) { scale-row @M, 1/@M[r;c], r }
sub clear-column ( @M,         \r,  \c  ) { shear-row @M, -@M[$_;c], $_, r for @M.keys.grep: * != r }

my @M = (
    [<  1   2   -1    -4 >],
    [<  2   3   -1   -11 >],
    [< -2   0   -3    22 >],
);

my $column-count = @M[0];
my $col = 0;
for @M.keys -> $row {
      reduce-row( @M, $row, $col );
    clear-column( @M, $row, $col );
    last if ++$col == $column-count;
}

say @$_».fmt(' %4g') for @M;
```

#### Output:
```
[    1     0     0    -8]
[    0     1     0     1]
[    0     0     1    -2]
```


### Row operations, object-oriented code



The same code as previous section, recast into OO. Also, scale and shear are recast as unscale and unshear, which fit the problem better.

```perl
class Matrix is Array {
    method  unscale-row ( @M: \scale, \row       ) { @M[row] =            @M[row] »/» scale }
    method  unshear-row ( @M: \scale, \r1,  \r2  ) { @M[r1]  = @M[r1] »-» @M[r2]  »×» scale }
    method   reduce-row ( @M:         \row, \col ) { @M.unscale-row( @M[row;col], row ) }
    method clear-column ( @M:         \row, \col ) { @M.unshear-row( @M[$_;col], $_, row ) for @M.keys.grep: * != row }

    method reduced-row-echelon-form ( @M: ) {
        my $column-count =  @M[0];
        my $col = 0;
        for @M.keys -> $row {
            @M.reduce-row(   $row, $col );
            @M.clear-column( $row, $col );
            return if ++$col == $column-count;
        }
    }
}

my $M = Matrix.new(
    [<  1   2   -1    -4 >],
    [<  2   3   -1   -11 >],
    [< -2   0   -3    22 >],
);

$M.reduced-row-echelon-form;
say @$_».fmt(' %4g') for @$M;
```

#### Output:
```
[    1     0     0    -8]
[    0     1     0     1]
[    0     0     1    -2]
```
