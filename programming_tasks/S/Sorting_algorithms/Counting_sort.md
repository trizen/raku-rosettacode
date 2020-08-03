[1]: https://rosettacode.org/wiki/Sorting_algorithms/Counting_sort

# [Sorting algorithms/Counting sort][1]

```raku
sub counting-sort (@ints) {
    my $off = @ints.min;
    (my @counts)[$_ - $off]++ for @ints;
    flat @counts.kv.map: { ($^k + $off) xx ($^v // 0) }
}
 
# Testing:
constant @age-range = 2 .. 102;
my @ages = @age-range.roll(50);
say @ages.&counting-sort;
say @ages.sort;
 
say @ages.&counting-sort.join eq @ages.sort.join ?? 'ok' !! 'not ok';
```

#### Output:
```
(5 5 5 7 9 17 19 19 20 21 25 27 28 30 32 34 38 40 41 45 48 49 50 51 53 54 55 56 59 62 65 66 67 69 70 73 74 81 83 85 87 91 91 93 94 96 99 99 100 101)
(5 5 5 7 9 17 19 19 20 21 25 27 28 30 32 34 38 40 41 45 48 49 50 51 53 54 55 56 59 62 65 66 67 69 70 73 74 81 83 85 87 91 91 93 94 96 99 99 100 101)
ok
```