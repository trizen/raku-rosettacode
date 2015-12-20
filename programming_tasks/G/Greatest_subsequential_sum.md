[1]: http://rosettacode.org/wiki/Greatest_subsequential_sum

# [Greatest subsequential sum][1]

```perl6
sub maxsubseq (*@a) {
    my ($start, $end, $sum, $maxsum) = -1, -1, 0, 0;
    for @a.kv -> $i, $x {
        $sum += $x;
        if $maxsum < $sum {
            ($maxsum, $end) = $sum, $i;
        }
        elsif $sum < 0 {
            ($sum, $start) = 0, $i;
        }
    }
    return @a[$start ^.. $end];
}
```


Another solution, not translated from any other language:



For each starting position, we calculate all the subsets starting at that position.
They are combined with the best subset ($max\_subset) from previous loops, to form (@subsets).
The best of those @subsets is saved at the new $max\_subset.



Consuming the array (.shift) allows us to skip tracking the starting point; it is always 0.



The empty sequence is used to initialize $max\_subset, which fufills the "all negative" requirement of the problem.



Note that once the triangular comma bug is resolved, the inner-loop subset calculation line can be shortened to "my @subsets = [\,] @a;".

```perl6
sub max_sub-seq ( *@a ) {
 
    my $max_subset = [];
    while @a {
        my @subsets = @a.keys.map: { [ @a[0..$_] ] };
        @subsets.push($max_subset);
        $max_subset = @subsets.max: { [+] .list };
        @a.shift;
    }
 
    return $max_subset;
}
 
max_sub-seq( -1, -2,  3,  5,  6, -2, -1,  4, -4,  2, -1 ).perl.say;
max_sub-seq( -2, -2, -1,  3,  5,  6, -1,  4, -4,  2, -1 ).perl.say;
max_sub-seq( -2, -2, -1, -3, -5, -6, -1, -4, -4, -2, -1 ).perl.say;
```

#### Output:
```
[3, 5, 6, -2, -1, 4]
[3, 5, 6, -1, 4]
[]
```