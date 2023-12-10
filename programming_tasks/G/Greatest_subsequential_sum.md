[1]: https://rosettacode.org/wiki/Greatest_subsequential_sum

# [Greatest subsequential sum][1]



```perl
sub max-subseq (*@a) {
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
They are combined with the best subset ($max-subset) from previous loops, to form (@subsets).
The best of those @subsets is saved at the new $max-subset.



Consuming the array (.shift) allows us to skip tracking the starting point; it is always 0.



The empty sequence is used to initialize $max-subset, which fulfils the "all negative" requirement of the problem.

```perl
sub max-subseq ( *@a ) {

    my $max-subset = ();
    while @a {
        my @subsets = [\,] @a;
        @subsets.push: $max-subset;
        $max-subset = @subsets.max: { [+] .list };
        @a.shift;
    }

    return $max-subset;
}

max-subseq( -1, -2,  3,  5,  6, -2, -1,  4, -4,  2, -1 ).say;
max-subseq( -2, -2, -1,  3,  5,  6, -1,  4, -4,  2, -1 ).say;
max-subseq( -2, -2, -1, -3, -5, -6, -1, -4, -4, -2, -1 ).say;
```

#### Output:
```
(3 5 6 -2 -1 4)
(3 5 6 -1 4)
()
```
