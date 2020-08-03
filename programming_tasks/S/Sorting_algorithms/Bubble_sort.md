[1]: https://rosettacode.org/wiki/Sorting_algorithms/Bubble_sort

# [Sorting algorithms/Bubble sort][1]

```raku
sub bubble_sort (@a) {
    for ^@a -> $i {
        for $i ^..^ @a -> $j {
            @a[$j] < @a[$i] and @a[$i, $j] = @a[$j, $i];
        }
    }
}
```