[1]: https://rosettacode.org/wiki/Sattolo_cycle

# [Sattolo cycle][1]

This modifies the array passed as argument, in-place.

```raku
sub sattolo-cycle (@array) {
    for reverse 1 .. @array.end -> $i {
        my $j = (^$i).pick;
        @array[$j, $i] = @array[$i, $j];
    }
}
```