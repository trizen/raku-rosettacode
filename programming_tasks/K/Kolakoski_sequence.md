[1]: https://rosettacode.org/wiki/Kolakoski_sequence

# [Kolakoski sequence][1]

```raku
sub kolakoski (*@seed) {
    my $k = @seed[0] == 1 ?? 1 !! 0;
    my @k = flat @seed[0] == 1 ?? (1, @seed[1] xx @seed[1]) !! @seed[0] xx @seed[0],
      { $k++; @seed[$k % @seed] xx @k[$k] } … *
}
 
sub rle (*@series) { @series.join.subst(/((.)$0*)/, -> { $0.chars }, :g).comb».Int }
 
# Testing
for [1, 2], 20,
    [2, 1], 20,
    [1, 3, 1, 2], 30,
    [1, 3, 2, 1], 30
  -> @seed, $terms {
    say "\n## $terms members of the series generated from { @seed.perl } is:\n   ",
    my @kolakoski = kolakoski(@seed)[^$terms];
    my @rle = rle @kolakoski;
    say "   Looks like a Kolakoski sequence?: ", @rle[*] eqv @kolakoski[^@rle];
}
```

#### Output:
```
## 20 members of the series generated from [1, 2] is:
   [1 2 2 1 1 2 1 2 2 1 2 2 1 1 2 1 1 2 2 1]
   Looks like a Kolakoski sequence?: True

## 20 members of the series generated from [2, 1] is:
   [2 2 1 1 2 1 2 2 1 2 2 1 1 2 1 1 2 2 1 2]
   Looks like a Kolakoski sequence?: True

## 30 members of the series generated from [1, 3, 1, 2] is:
   [1 3 3 3 1 1 1 2 2 2 1 3 1 2 2 1 1 3 3 1 2 2 2 1 3 3 1 1 2 1]
   Looks like a Kolakoski sequence?: True

## 30 members of the series generated from [1, 3, 2, 1] is:
   [1 3 3 3 2 2 2 1 1 1 1 1 3 3 2 2 1 1 3 2 1 1 1 1 3 3 3 2 2 1]
   Looks like a Kolakoski sequence?: False
```