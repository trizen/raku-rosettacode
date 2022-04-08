[1]: https://rosettacode.org/wiki/Last_list_item

# [Last list item][1]

Uses no sorting; does not modify overall list order while processing.

```perl
say ' Original ', my @list = 6, 81, 243, 14, 25, 49, 123, 69, 11;
 
say push @list: get(min @list) + get(min @list) while @list > 1;
 
sub get ($min) {
    @list.splice: @list.first(* == $min, :k), 1;
    printf " %3d ", $min;
    $min;
}
```

#### Output:
```
 Original [6 81 243 14 25 49 123 69 11]
   6   11 [81 243 14 25 49 123 69 17]
  14   17 [81 243 25 49 123 69 31]
  25   31 [81 243 49 123 69 56]
  49   56 [81 243 123 69 105]
  69   81 [243 123 105 150]
 105  123 [243 150 228]
 150  228 [243 378]
 243  378 [621]
```
