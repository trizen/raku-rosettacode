[1]: https://rosettacode.org/wiki/Sequence:_smallest_number_with_exactly_n_divisors

# [Sequence: smallest number with exactly n divisors][1]



```perl
sub div-count (\x) {
    return 2 if x.is-prime;
    +flat (1 .. x.sqrt.floor).map: -> \d {
        unless x % d { my \y = x div d; y == d ?? y !! (y, d) }
    }
}

my $limit = 15;

put "First $limit terms of OEIS:A005179";
put (1..$limit).map: -> $n { first { $n == .&div-count }, 1..Inf };
```

#### Output:
```
First 15 terms of OEIS:A005179
1 2 4 6 16 12 64 24 36 48 1024 60 4096 192 144
```
