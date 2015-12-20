[1]: http://rosettacode.org/wiki/Sorting_algorithms/Counting_sort

# [Sorting algorithms/Counting sort][1]

```perl
sub counting-sort (@ints) {
    my $off = @ints.min;
    (my @counts)[$_ - $off]++ for @ints;
    @counts.kv.map: { ($^k + $off) xx ($^v // 0) }
}
```


Testing:

```perl
constant @age-range = 2 .. 102;
my @ages = @age-range.roll(50);
say @ages.&counting-sort ~~ @ages.sort ?? 'ok' !! 'not ok';
```

#### Output:
```
ok
```