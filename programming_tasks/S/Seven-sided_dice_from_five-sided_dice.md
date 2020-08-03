[1]: https://rosettacode.org/wiki/Seven-sided_dice_from_five-sided_dice

# [Seven-sided dice from five-sided dice][1]

```raku
my $d5 = 1..5;
sub d5() { $d5.roll; }  # 1d5
 
sub d7() {
    my $flat = 21;
    $flat = 5 * d5() - d5() until $flat < 21;
    $flat % 7 + 1;
}
 
# Testing
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
1       143088  +0.16%
2       143598  +0.52%
3       141741  -0.78%
4       142832  -0.02%
5       143040  +0.13%
6       142988  +0.09%
7       142713  -0.10%
```