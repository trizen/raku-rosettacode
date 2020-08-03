[1]: https://rosettacode.org/wiki/Verify_distribution_uniformity/Naive

# [Verify distribution uniformity/Naive][1]

Since the tested function is rolls of a 7 sided die, the test numbers are magnitudes of 10<sup>x</sup> bumped up to the closest multiple of 7. This reduces spurious error from there not being an integer expected value.

```perl
my $d7 = 1..7;
sub roll7 { $d7.roll };
 
my $threshold = 3;
 
for 14, 105, 1001, 10003, 100002, 1000006 ->  $n
  { dist( $n, $threshold,  &roll7 ) };
 
 
sub dist ( $n is copy, $threshold, &producer ) {
    my @dist;
    my $expect = $n / 7;
    say "Expect\t",$expect.fmt("%.3f");
 
    loop ($_ = $n; $n; --$n) { @dist[&producer()]++; }
 
    for @dist.kv -> $i, $v is copy {
        next unless $i;
        $v //= 0;
        my $pct = ($v - $expect)/$expect*100;
        printf "%d\t%d\t%+.2f%% %s\n", $i, $v, $pct,
          ($pct.abs > $threshold ?? '- skewed' !! '');
    }
    say '';
}
```


Sample output:


#### Output:
```
Expect  2.000
1       2       +0.00%
2       3       +50.00% - skewed
3       2       +0.00%
4       2       +0.00%
5       3       +50.00% - skewed
6       0       -100.00% - skewed
7       2       +0.00%

Expect  15.000
1       15      +0.00%
2       17      +13.33% - skewed
3       13      -13.33% - skewed
4       16      +6.67% - skewed
5       14      -6.67% - skewed
6       16      +6.67% - skewed
7       14      -6.67% - skewed

Expect  143.000
1       134     -6.29% - skewed
2       142     -0.70%
3       141     -1.40%
4       137     -4.20% - skewed
5       142     -0.70%
6       170     +18.88% - skewed
7       135     -5.59% - skewed

Expect  1429.000
1       1396    -2.31%
2       1468    +2.73%
3       1405    -1.68%
4       1442    +0.91%
5       1453    +1.68%
6       1417    -0.84%
7       1422    -0.49%

Expect  14286.000
1       14222   -0.45%
2       14320   +0.24%
3       14326   +0.28%
4       14425   +0.97%
5       14140   -1.02%
6       14275   -0.08%
7       14294   +0.06%

Expect  142858.000
1       142510  -0.24%
2       142436  -0.30%
3       142438  -0.29%
4       143152  +0.21%
5       142905  +0.03%
6       143232  +0.26%
7       143333  +0.33%
```