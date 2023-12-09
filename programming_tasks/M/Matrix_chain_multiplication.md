[1]: https://rosettacode.org/wiki/Matrix_chain_multiplication

# [Matrix chain multiplication][1]

```perl
# Reference:
# "Find the optimal way to multiply a chain of matrices. " was the first tasks at
# "masak's Perl 6 Coding Contest 2010". Here is the task definition
# http://strangelyconsistent.org/p6cc2010/p1.zip
#
# There are five finalist entries on the task, here are the relevant codes and contest host's comments
# 1) http://strangelyconsistent.org/p6cc2010/p1-colomon/
# 2) http://strangelyconsistent.org/p6cc2010/p1-fox/
# 3) http://strangelyconsistent.org/p6cc2010/p1-moritz/
# 4) http://strangelyconsistent.org/p6cc2010/p1-matthias/
# 5) http://strangelyconsistent.org/p6cc2010/p1-util/
#
# All entries passed perl6.d -c but during run time only the 3) entry (from moritz) produced the correct results. 
 
#!/usr/bin/env perl6
use v6;
# after https://en.wikipedia.org/wiki/Matrix_chain_multiplication#A_Dynamic_Programming_Algorithm
sub matrix-mult-chaining(@dimensions) {
    # @cp has a dual function: the upper triangle of the diagonal matrix
    # stores the cost (c) for mulplying matrices $i and $j in @cp[$j][$i],
    # $j > $i
    # the lower triangle stores the path (p) that was used for the lowest cost
    # multiplication to get from $i to $j.
    #
    #    wikipedia         this program
    #    m[i][j]           @cp[$j][$i]
    #    s[i][j]           @cp[$i][$j]
    #
    # it makes the code harder to read, but hey, it saves memory!
    my @cp;
    # a matrix never needs to be multiplied with itself,
    # so it has cost 0
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
                    # cost
                    @cp[$end][$start] = $new-cost;
                    # path
                    @cp[$start][$end] = $step;
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
    return  gather { find-path(0, $n - 1) }.join;
}
multi MAIN {
    my $line = get;
    my @dimensions = $line.comb: /\d+/;
    if @dimensions < 2 {
        say "Input must consist of at least two integers.";
    } else {
        say matrix-mult-chaining(@dimensions);
    }
}
multi MAIN ('test') {
    use Test;
    plan *;
    is matrix-mult-chaining(<10 30 5 60>), '((A1A2)A3)', 'wp example';
    # http://www.cs.auckland.ac.nz/~jmor159/PLDS210/mat_chain.html
    is matrix-mult-chaining(<10 100 5 50>), '((A1A2)A3)', 'auckland';
    # an example verified by http://modoogle.com/matrixchainorder/
    is matrix-mult-chaining(<6 2 5 1 6 3 9 1 25 18>),
            '((A1((A2A3)((A4A5)(A6A7))))(A8A9))', 'modoogle';
    # done_testing;
}
```

#### Output:
```
./mcm3.p6
1, 5, 25, 30, 100, 70, 2, 1, 100, 250, 1, 1000, 2
((((((((A1A2)A3)A4)A5)A6)A7)(A8(A9A10)))(A11A12))
```

#### Output:
```
./mcm3.p6
1000, 1, 500, 12, 1, 700, 2500, 3, 2, 5, 14, 10
(A1((((((A2A3)A4)(((A5A6)A7)A8))A9)A10)A11))
```