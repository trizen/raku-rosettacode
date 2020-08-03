[1]: https://rosettacode.org/wiki/Sorting_algorithms/Bogosort

# [Sorting algorithms/Bogosort][1]

```raku
sub bogosort (@list is copy) {
    @list .= pick(*) until [<=] @list;
    return @list;
}
 
my @nums = (^5).map: { rand };
say @nums.sort.Str eq @nums.&bogosort.Str ?? 'ok' !! 'not ok';
 
```