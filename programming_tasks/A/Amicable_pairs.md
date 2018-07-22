[1]: https://rosettacode.org/wiki/Amicable_pairs

# [Amicable pairs][1]

```perl
sub propdivsum (\x) {
    my @l = x > 1, gather for 2 .. x.sqrt.floor -> \d {
        my \y = x div d;
        if y * d == x { take d; take y unless y == d }
    }
    [+] gather @l.deepmap(*.take);
}
 
for 1..20000 -> $i {
    my $j = propdivsum($i);
    say "$i $j" if $j > $i and $i == propdivsum($j);
}
```

#### Output:
```
220 284
1184 1210
2620 2924
5020 5564
6232 6368
10744 10856
12285 14595
17296 18416
```