[1]: https://rosettacode.org/wiki/Gauss-Jordan_matrix_inversion

# [Gauss-Jordan matrix inversion][1]

Uses bits and pieces from other tasks, [Reduced row echelon form](https://rosettacode.org/wiki/Reduced_row_echelon_form#Perl_6) primarily.

```perl
sub gauss-jordan-invert (@m where *.&is-square) {
    ^@m .map: { @m[$_].append: identity(+@m)[$_] };
    @m.&rref[*]»[+@m .. *];
}
 
sub is-square (@m) { so @m == all @m[*] }
 
sub identity ($n) { [ 1, |(0 xx $n-1) ], *.rotate(-1) ... *.tail }
 
# reduced row echelon form (Gauss-Jordan elimination)
sub rref (@m) {
    return unless @m;
    my ($lead, $rows, $cols) = 0, +@m, +@m[0];
 
    for ^$rows -> $r {
        $lead < $cols or return @m;
        my $i = $r;
        until @m[$i;$lead] {
            ++$i == $rows or next;
            $i = $r;
            ++$lead == $cols and return @m;
        }
        @m[$i, $r] = @m[$r, $i] if $r != $i;
        my $lv = @m[$r;$lead];
        @m[$r] »/=» $lv;
        for ^$rows -> $n {
            next if $n == $r;
            @m[$n] »-=» @m[$r] »*» (@m[$n;$lead] // 0);
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
    $_».&rat-or-int.fmt(" %6s").put for @array;
}
 
my @tests = (
    [
      [ 1, 2, 3 ],
      [ 4, 1, 6 ],
      [ 7, 8, 9 ]
    ],
    [
      [ 2, -1,  0 ],
      [-1,  2, -1 ],
      [ 0, -1,  2 ]
    ],
    [
      [ -1, -2, 3, 2 ],
      [ -4, -1, 6, 2 ],
      [  7, -8, 9, 1 ],
      [  1, -2, 1, 3 ]
    ],
);
 
 
for @tests -> @matrix {
    say_it( 'Original Matrix:', @matrix );
    say_it( 'Gauss-Jordan Inverted Matrix:', gauss-jordan-invert @matrix );
    say "\n";
}
 
```

#### Output:
```
Original Matrix:
      1       2       3
      4       1       6
      7       8       9

Gauss-Jordan Inverted Matrix:
 -13/16     1/8    3/16
    1/8    -1/4     1/8
  25/48     1/8   -7/48



Original Matrix:
      2      -1       0
     -1       2      -1
      0      -1       2

Gauss-Jordan Inverted Matrix:
    3/4     1/2     1/4
    1/2       1     1/2
    1/4     1/2     3/4



Original Matrix:
     -1      -2       3       2
     -4      -1       6       2
      7      -8       9       1
      1      -2       1       3

Gauss-Jordan Inverted Matrix:
 -21/23   17/69  13/138   19/46
 -38/23   15/23    1/23   15/23
 -16/23   25/69  11/138    9/46
 -13/23   16/69   -2/69   13/23
```