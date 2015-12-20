[1]: http://rosettacode.org/wiki/Seven-sided_dice_from_five-sided_dice

# [Seven-sided dice from five-sided dice][1]

Since rakudo is still pretty slow, we've done some interesting bits of optimization.
We factor out the range object construction so that it doesn't have to be recreated each time, and we sneakily <em>subtract</em> the 1's from the 5's, which takes us back to 0 based without having to subtract 6.

```perl6
my $d5 = 1..5;
sub d5() { $d5.roll; }  # 1d5
 
sub d7() {
    my $flat = 21;
    $flat = 5 * d5() - d5() until $flat < 21;
    $flat % 7 + 1;
}
```


Here's the test. We use a C-style for loop, except it's named `loop`, because it's currently faster than the other loops--and, er, doesn't segfault the GC on a million iterations...

```perl6
my @dist;
my $n = 1_000_000;
my $expect = $n / 7;
 
loop ($_ = $n; $n; --$n) { @dist[d7()]++; }
 
say "Expect\t",$expect.fmt("%.3f");
for @dist.kv -> $i, $v {
    say "$i\t$v\t" ~ (($v - $expect)/$expect*100).fmt("%+.2f%%") if $v;
}
```

#### Output:
```
Expect  142857.143
1       142835  -0.02%
2       143021  +0.11%
3       142771  -0.06%
4       142706  -0.11%
5       143258  +0.28%
6       142485  -0.26%
7       142924  +0.05%
```