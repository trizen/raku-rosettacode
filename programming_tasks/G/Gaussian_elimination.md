[1]: https://rosettacode.org/wiki/Gaussian_elimination

# [Gaussian elimination][1]

Gaussian elimination results in a matrix in row echelon form. Gaussian elimination with back-substitution (also known as Gauss-Jordan elimination) results in a matrix in reduced row echelon form. That being the case, we can reuse much of the code from the [Reduced row echelon form](https://rosettacode.org/wiki/Reduced_row_echelon_form) task. Perl&#160;6 stores and does calculations on decimal numbers within its limit of precision using Rational numbers by default, meaning the calculations are exact.

```perl
sub gauss-jordan-solve (@a, @b) {
    @b.kv.map: { @a[$^k].append: $^v };
    @a.&rref[*]»[*-1];
}
 
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
 
sub say-it ($message, @array, $fmt = " %8s") {
    say "\n$message";
    $_».&rat-or-int.fmt($fmt).put for @array;
}
 
my @a = (
    [ 1.00, 0.00, 0.00,  0.00,  0.00,   0.00 ],
    [ 1.00, 0.63, 0.39,  0.25,  0.16,   0.10 ],
    [ 1.00, 1.26, 1.58,  1.98,  2.49,   3.13 ],
    [ 1.00, 1.88, 3.55,  6.70, 12.62,  23.80 ],
    [ 1.00, 2.51, 6.32, 15.88, 39.90, 100.28 ],
    [ 1.00, 3.14, 9.87, 31.01, 97.41, 306.02 ],
);
my @b = ( -0.01, 0.61, 0.91, 0.99, 0.60, 0.02 );
 
say-it 'A matrix:', @a, "%6.2f";
say-it 'or, A in exact rationals:', @a;
say-it 'B matrix:', @b, "%6.2f";
say-it 'or, B in exact rationals:', @b;
say-it 'x matrix:', (my @gj = gauss-jordan-solve @a, @b), "%16.12f";
say-it 'or, x in exact rationals:', @gj, "%28s";
 
```

#### Output:
```
A matrix:
  1.00   0.00   0.00   0.00   0.00   0.00
  1.00   0.63   0.39   0.25   0.16   0.10
  1.00   1.26   1.58   1.98   2.49   3.13
  1.00   1.88   3.55   6.70  12.62  23.80
  1.00   2.51   6.32  15.88  39.90 100.28
  1.00   3.14   9.87  31.01  97.41 306.02

or, A in exact rationals:
        1         0         0         0         0         0
        1    63/100    39/100       1/4      4/25      1/10
        1     63/50     79/50     99/50   249/100   313/100
        1     47/25     71/20     67/10    631/50     119/5
        1   251/100    158/25    397/25    399/10   2507/25
        1    157/50   987/100  3101/100  9741/100  15301/50

B matrix:
 -0.01
  0.61
  0.91
  0.99
  0.60
  0.02

or, B in exact rationals:
   -1/100
   61/100
   91/100
   99/100
      3/5
     1/50

x matrix:
 -0.010000000000
  1.602790394502
 -1.613203059906
  1.245494121371
 -0.490989719585
  0.065760696175

or, x in exact rationals:
                      -1/100
   655870882787/409205648497
  -660131804286/409205648497
   509663229635/409205648497
  -200915766608/409205648497
    26909648324/409205648497
```