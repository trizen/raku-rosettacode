[1]: https://rosettacode.org/wiki/Sequence:_smallest_number_greater_than_previous_term_with_exactly_n_divisors

# [Sequence: smallest number greater than previous term with exactly n divisors][1]

```perl
sub div-count (\x) {
    return 2 if x.is-prime;
    +flat (1 .. x.sqrt.floor).map: -> \d {
        unless x % d { my \y = x div d; y == d ?? y !! (y, d) }
    }
}
 
my $limit = 15;
 
my $m = 1;
put "First $limit terms of OEIS:A069654";
put (1..$limit).map: -> $n { my $ = $m = first { $n == .&div-count }, $m..Inf };
 
```

#### Output:
```
First 15 terms of OEIS:A069654
1 2 4 6 16 18 64 66 100 112 1024 1035 4096 4288 4624
```