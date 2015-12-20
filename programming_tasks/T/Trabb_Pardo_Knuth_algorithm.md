[1]: http://rosettacode.org/wiki/Trabb_Pardo–Knuth_algorithm

# [Trabb Pardo–Knuth algorithm][1]

```perl
my @nums = prompt("Please type 11 space-separated numbers: ").words
    until @nums == 11;
for @nums.reverse -> $n {
    my $r = $n.abs.sqrt + 5 * $n ** 3;
    say "$n\t{ $r > 400 ?? 'Urk!' !! $r }";
}
```

#### Output:
```
Please type 11 space-separated numbers: 10 -1 1 2 3 4 4.3 4.305 4.303 4.302 4.301
4.301   399.88629974772681
4.302   Urk!
4.303   Urk!
4.305   Urk!
4.3     399.60864413533278
4       322
3       136.73205080756887
2       41.414213562373092
1       6
-1      -4
10      Urk!
```