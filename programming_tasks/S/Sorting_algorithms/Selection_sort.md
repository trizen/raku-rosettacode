[1]: http://rosettacode.org/wiki/Sorting_algorithms/Selection_sort

# [Sorting algorithms/Selection sort][1]

```perl6
sub selection_sort ( @a is copy ) {
    for 0 ..^ @a.end -> $i {
        my $min = [ $i+1 .. @a.end ].min: { @a[$_] };
        @a[$i, $min] = @a[$min, $i] if @a[$i] > @a[$min];
    }
    return @a;
}
 
my @data = 22, 7, 2, -5, 8, 4;
say 'input  = ' ~ @data;
say 'output = ' ~ @data.&selection_sort;
 
```

#### Output:
```
input  = 22 7 2 -5 8 4
output = -5 2 4 7 8 22
```