[1]: http://rosettacode.org/wiki/Reduced_row_echelon_form

# [Reduced row echelon form][1]

```perl
sub rref (@m) {
    @m or return;
    my ($lead, $rows, $cols) = 0, +@m, +@m[0];
 
    for ^$rows -> $r {
        return @m if $lead >= $cols;
        my $i = $r;
 
        until @m[$i][$lead] {
            ++$i == $rows or next;
            $i = $r;
            ++$lead == $cols and return @m;
        }
 
        @m[$i, $r] = @m[$r, $i];
 
        my $lv = @m[$r][$lead];
        @m[$r] »/=» $lv;
 
        for ^$rows -> $n {
            next if $n == $r;
            @m[$n] »-=» @m[$r] »*» @m[$n][$lead];
        }
        ++$lead;
    }
    @m
}
 
sub rat-or-int ($num) {
    return $num unless $num ~~ Rat;
    return $num.narrow if $num.narrow.WHAT ~~ Int;
    $num.nude.join: '/';
}
 
sub say_it ($message, @array) {
    say "\n$message";
    $_».&rat-or-int.fmt(" %5s").say for @array;
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


Perl 6 handles rational numbers internally as a ratio of two integers 
to maintain precision. 
For some situations it is useful to return the ratio 
rather than the floating point result.


#### Output:
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


Re-implemented without the pseudocode, expressed as elementary matrix row operations. See 
[http://unapologetic.wordpress.com/2009/08/27/elementary-row-and-column-operations/](http://unapologetic.wordpress.com/2009/08/27/elementary-row-and-column-operations/)
and
[http://unapologetic.wordpress.com/2009/09/03/reduced-row-echelon-form/](http://unapologetic.wordpress.com/2009/09/03/reduced-row-echelon-form/)



First, a procedural version:

```perl
sub swap_rows    ( @M,         $r1, $r2 ) { @M[ $r1, $r2 ] = @M[ $r2, $r1 ] };
sub scale_row    ( @M, $scale, $r       ) { @M[$r]  =              @M[$r]  »*» $scale   };
sub shear_row    ( @M, $scale, $r1, $r2 ) { @M[$r1] = @M[$r1].list »+» ( @M[$r2] »*» $scale ) };
sub reduce_row   ( @M,         $r,  $c  ) { scale_row( @M, 1/@M[$r][$c], $r ) };
sub clear_column ( @M,         $r,  $c  ) {
    for @M.keys.grep( * != $r ) -> $row_num {
        shear_row( @M, -1*@M[$row_num][$c], $row_num, $r );
    }
}
 
my @M = (
    [<  1   2   -1    -4 >],
    [<  2   3   -1   -11 >],
    [< -2   0   -3    22 >],
);
 
my $column_count = +@( @M[0] );
 
my $current_col = 0;
while all( @M».[$current_col] ) == 0 {
    $current_col++;
    return if $current_col == $column_count; # Matrix was all-zeros.
}
 
for @M.keys -> $current_row {
    reduce_row(   @M, $current_row, $current_col );
    clear_column( @M, $current_row, $current_col );
    $current_col++;
    return if $current_col == $column_count;
}
 
say @($_)».fmt(' %4g') for @M;
```


And the same code, recast into OO. Also, scale and shear are recast as unscale and unshear, which fit the problem better.

```perl
class Matrix is Array {
    method unscale_row ( @M: $scale, $row ) {
        @M[$row] = @M[$row] »/» $scale;
    }
    method unshear_row ( @M: $scale, $r1, $r2 ) {
        @M[$r1] = @M[$r1] »-» ( @M[$r2] »*» $scale );
    }
    method reduce_row ( @M: $row, $col ) {
        @M.unscale_row( @M[$row][$col], $row );
    }
    method clear_column ( @M: $row, $col ) {
        for @M.keys.grep( * != $row ) -> $scanning_row {
            @M.unshear_row( @M[$scanning_row][$col], $scanning_row, $row );
        }
    }
    method reduced_row_echelon_form ( @M: ) {
        my $column_count = +@( @M[0] );
 
        my $current_col = 0;
        # Skip past all-zero columns.
        while all( @M».[$current_col] ) == 0 {
            $current_col++;
            return if $current_col == $column_count; # Matrix was all-zeros.
        }
 
        for @M.keys -> $current_row {
            @M.reduce_row(   $current_row, $current_col );
            @M.clear_column( $current_row, $current_col );
            $current_col++;
            return if $current_col == $column_count;
        }
    }
}
 
my $M = Matrix.new(
    [<  1   2   -1    -4 >],
    [<  2   3   -1   -11 >],
    [< -2   0   -3    22 >],
);
 
$M.reduced_row_echelon_form;
 
say @($_)».fmt(' %4g') for @($M);
```


Note that both versions can be simplified using Z+=, Z-=, X\*=, 
and X/= to scale and shear. 
Currently, Rakudo has a bug related to Xop= and Zop=.



Note that the negative zeros in the output are innocuous, 
and also occur in the Perl 5 version.