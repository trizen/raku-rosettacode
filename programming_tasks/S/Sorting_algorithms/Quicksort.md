[1]: https://rosettacode.org/wiki/Sorting_algorithms/Quicksort

# [Sorting algorithms/Quicksort][1]

```perl
# Empty list sorts to the empty list
 multi quicksort([]) { () }
 
 # Otherwise, extract first item as pivot...
 multi quicksort([$pivot, *@rest]) {
     # Partition.
     my $before := @rest.grep(* before $pivot);
     my $after  := @rest.grep(* !before $pivot);
 
     # Sort the partitions.
     flat quicksort($before), $pivot, quicksort($after)
 }
```


Note that `$before` and `$after` are bound to lazy lists, so the partitions can (at least in theory) be sorted in parallel.