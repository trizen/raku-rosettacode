[1]: https://rosettacode.org/wiki/Van_Eck_sequence

# [Van Eck sequence][1]

There is not **a** Van Eck sequence, rather a series of related sequences that differ in their starting value. This task is nominally for the sequence starting with the value 0. This Perl 6 implementation will handle any integer starting value.



Specifically handles:



among others.



Implemented as lazy, extendable lists.

```raku
sub n-van-ecks ($init) {
    $init, -> $i, {
        state %v;
        state $k;
        $k++;
        my $t  = %v{$i}.defined ?? $k - %v{$i} !! 0;
        %v{$i} = $k;
        $t
    } ... *
}
 
for <
    A181391 0
    A171911 1
    A171912 2
    A171913 3
    A171914 4
    A171915 5
    A171916 6
    A171917 7
    A171918 8
> -> $seq, $start {
 
    my @seq = n-van-ecks($start);
 
    # The task
    put qq:to/END/
 
    Van Eck sequence OEIS:$seq; with the first term: $start
            First 10 terms: {@seq[^10]}
    Terms 991 through 1000: {@seq[990..999]}
    END
}
```

#### Output:
```
Van Eck sequence OEIS:A181391; with the first term: 0
        First 10 terms: 0 0 1 0 2 0 2 2 1 6
Terms 991 through 1000: 4 7 30 25 67 225 488 0 10 136


Van Eck sequence OEIS:A171911; with the first term: 1
        First 10 terms: 1 0 0 1 3 0 3 2 0 3
Terms 991 through 1000: 0 6 53 114 302 0 5 9 22 71


Van Eck sequence OEIS:A171912; with the first term: 2
        First 10 terms: 2 0 0 1 0 2 5 0 3 0
Terms 991 through 1000: 8 92 186 0 5 19 41 413 0 5


Van Eck sequence OEIS:A171913; with the first term: 3
        First 10 terms: 3 0 0 1 0 2 0 2 2 1
Terms 991 through 1000: 5 5 1 17 192 0 6 34 38 179


Van Eck sequence OEIS:A171914; with the first term: 4
        First 10 terms: 4 0 0 1 0 2 0 2 2 1
Terms 991 through 1000: 33 410 0 6 149 0 3 267 0 3


Van Eck sequence OEIS:A171915; with the first term: 5
        First 10 terms: 5 0 0 1 0 2 0 2 2 1
Terms 991 through 1000: 60 459 0 7 13 243 0 4 10 211


Van Eck sequence OEIS:A171916; with the first term: 6
        First 10 terms: 6 0 0 1 0 2 0 2 2 1
Terms 991 through 1000: 6 19 11 59 292 0 6 6 1 12


Van Eck sequence OEIS:A171917; with the first term: 7
        First 10 terms: 7 0 0 1 0 2 0 2 2 1
Terms 991 through 1000: 11 7 2 7 2 2 1 34 24 238


Van Eck sequence OEIS:A171918; with the first term: 8
        First 10 terms: 8 0 0 1 0 2 0 2 2 1
Terms 991 through 1000: 16 183 0 6 21 10 249 0 5 48
```
