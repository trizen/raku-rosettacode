[1]: https://rosettacode.org/wiki/Faulhaber's_triangle

# [Faulhaber's triangle][1]

```raku
# Helper subs
 
sub infix:<reduce> (\prev, \this) { this.key => this.key * (this.value - prev.value) }
 
sub next-bernoulli ( (:key($pm), :value(@pa)) ) {
    $pm + 1 => [ map *.value, [\reduce] ($pm + 2 ... 1) Z=> 1 / ($pm + 2), |@pa ]
}
 
constant bernoulli = (0 => [1.FatRat], &next-bernoulli ... *).map: { .value[*-1] };
 
sub binomial (Int $n, Int $p) { combinations($n, $p).elems }
 
sub asRat (FatRat $r) { $r ?? $r.denominator == 1 ?? $r.numerator !! $r.nude.join('/') !! 0 }
 
 
# The task
sub faulhaber_triangle ($p) { map { binomial($p + 1, $_) * bernoulli[$_] / ($p + 1) }, ($p ... 0) }
 
# First 10 rows of Faulhaber's triangle:
say faulhaber_triangle($_)».&asRat.fmt('%5s') for ^10;
say '';
 
# Extra credit:
my $p = 17;
my $n = 1000;
say sum faulhaber_triangle($p).kv.map: { $^value * $n**($^key + 1) }
```

#### Output:
```
    1
  1/2   1/2
  1/6   1/2   1/3
    0   1/4   1/2   1/4
-1/30     0   1/3   1/2   1/5
    0 -1/12     0  5/12   1/2   1/6
 1/42     0  -1/6     0   1/2   1/2   1/7
    0  1/12     0 -7/24     0  7/12   1/2   1/8
-1/30     0   2/9     0 -7/15     0   2/3   1/2   1/9
    0 -3/20     0   1/2     0 -7/10     0   3/4   1/2  1/10

56056972216555580111030077961944183400198333273050000
```