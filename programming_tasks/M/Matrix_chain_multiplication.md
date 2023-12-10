[1]: https://rosettacode.org/wiki/Matrix_chain_multiplication

# [Matrix chain multiplication][1]


This example is based on Moritz Lenz's code, written for Carl Mäsak's Perl 6 Coding Contest, in 2010. Slightly simplified, it fulfills the Rosetta Code task as well.

```perl
sub matrix-mult-chaining(@dimensions) {
    my @cp;
    # @cp has a dual function: 
    # * the upper triangle of the diagonal matrix stores the cost (c) for
    #   multiplying matrices $i and $j in @cp[$j][$i], where $j > $i
    # * the lower triangle stores the path (p) that was used for the lowest cost
    #   multiplication to get from $i to $j.

    # a matrix never needs to be multiplied with itself, so it has cost 0
    @cp[$_][$_] = 0 for @dimensions.keys;
    my @path;

    my $n = @dimensions.end;
    for 1 .. $n -> $chain-length {
        for 0 .. $n - $chain-length - 1 -> $start {
            my $end = $start + $chain-length;
            @cp[$end][$start] = Inf;  # until we find a better connection
            for $start .. $end - 1 -> $step {
                my $new-cost = @cp[$step][$start]
                             + @cp[$end][$step + 1]
                             + [*] @dimensions[$start, $step+1, $end+1];
                if $new-cost < @cp[$end][$start] {
                    @cp[$end][$start] = $new-cost; # cost
                    @cp[$start][$end] = $step;     # path
                }
            }
       }
    }

    sub find-path(Int $start, Int $end) {
        if $start == $end {
            take 'A' ~ ($start + 1);
        } else {
            take '(';
            find-path($start, @cp[$start][$end]);
            find-path(@cp[$start][$end] + 1, $end);
            take ')';
        }
    }
 
   return @cp[$n-1][0], gather { find-path(0, $n - 1) }.join;
}

say matrix-mult-chaining(<1 5 25 30 100 70 2 1 100 250 1 1000 2>);
say matrix-mult-chaining(<1000 1 500 12 1 700 2500 3 2 5 14 10>);
```

#### Output:
```
(38120 ((((((((A1A2)A3)A4)A5)A6)A7)(A8(A9A10)))(A11A12)))
(1773740 (A1((((((A2A3)A4)(((A5A6)A7)A8))A9)A10)A11)))
```
