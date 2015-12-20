[1]: http://rosettacode.org/wiki/Sorting_algorithms/Radix_sort

# [Sorting algorithms/Radix sort][1]

A base-10 radix sort, done on the string representation of the integers. Signs are handled by in-place reversal of the '-' bucket on the last iteration. (The sort in there is not cheating; it only makes sure we process the buckets in the right order, since `classify` might return the buckets in random order. It might be more efficient to create our own ordered buckets, but this is succinct.)

```perl
sub radsort (@ints) {
    my $maxlen = [max] @ints».chars;
    my @list = @ints».fmt("\%0{$maxlen}d");
 
    for reverse ^$maxlen -> $r {
        my @buckets = @list.classify( *.substr($r,1) ).sort: *.key;
        if !$r and @buckets[0].key eq '-' { @buckets[0].value .= reverse }
        @list = map *.value.values, @buckets;
    }
    @list».Int;
}
 
.say for radsort (-2_000 .. 2_000).roll(20);
```

#### Output:
```
-1585
-1427
-1228
-1067
-945
-657
-643
-232
-179
-28
37
411
488
509
716
724
1504
1801
1864
1939
```