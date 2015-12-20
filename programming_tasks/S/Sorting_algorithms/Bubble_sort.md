[1]: http://rosettacode.org/wiki/Sorting_algorithms/Bubble_sort

# [Sorting algorithms/Bubble sort][1]

```perl
sub bubble_sort (@a is rw) {
    for ^@a -> $i {
        for $i ^..^ @a -> $j {
            @a[$j] < @a[$i] and @a[$i, $j] = @a[$j, $i];
        }
    }
}
```