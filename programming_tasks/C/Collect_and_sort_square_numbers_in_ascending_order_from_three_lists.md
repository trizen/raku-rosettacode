[1]: https://rosettacode.org/wiki/Collect_and_sort_square_numbers_in_ascending_order_from_three_lists

# [Collect and sort square numbers in ascending order from three lists][1]

```perl
my $s = cache sort ( my $l = ( cache flat

[3,4,34,25,9,12,36,56,36],
[2,8,81,169,34,55,76,49,7],
[75,121,75,144,35,16,46,35]

)).grep: * ∈ cache {$++²}…*>$l.max;

put 'Added - Sorted';
put ' ' ~ $s.sum ~ '  ' ~ $s.gist;
```

#### Output:
```
Added - Sorted
 690  (4 9 16 25 36 36 49 81 121 144 169)
```
