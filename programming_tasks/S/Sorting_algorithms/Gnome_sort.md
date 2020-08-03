[1]: https://rosettacode.org/wiki/Sorting_algorithms/Gnome_sort

# [Sorting algorithms/Gnome sort][1]

```raku
sub gnome_sort (@a) {
    my ($i, $j) = 1, 2;
    while $i < @a {
        if @a[$i - 1] <= @a[$i] {
            ($i, $j) = $j, $j + 1;
        }
        else {
            (@a[$i - 1], @a[$i]) = @a[$i], @a[$i - 1];
            $i--;
            ($i, $j) = $j, $j + 1 if $i == 0;
        }
    }
}
```


my @n = (1..10).roll(20);
say @n.&amp;gnome\_sort ~~ @n.sort&#160;?? 'ok'&#160;!! 'not ok';