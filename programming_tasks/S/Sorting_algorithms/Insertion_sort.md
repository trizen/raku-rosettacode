[1]: https://rosettacode.org/wiki/Sorting_algorithms/Insertion_sort

# [Sorting algorithms/Insertion sort][1]



```perl
sub insertion_sort ( @a is copy ) {
    for 1 .. @a.end -> $i {
        my $value = @a[$i];
        my $j;
        loop ( $j = $i-1; $j >= 0 and @a[$j] > $value; $j-- ) {
            @a[$j+1] = @a[$j];
        }
        @a[$j+1] = $value;
    }
    return @a;
}

my @data = 22, 7, 2, -5, 8, 4;
say 'input  = ' ~ @data;
say 'output = ' ~ @data.&insertion_sort;
```

#### Output:
```
input  = 22 7 2 -5 8 4
output = -5 2 4 7 8 22
```
