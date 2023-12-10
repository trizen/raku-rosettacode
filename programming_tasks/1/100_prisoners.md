[1]: https://rosettacode.org/wiki/100_prisoners

# [100 prisoners][1]





Accepts command line parameters to modify the number of prisoners and the number of simulations to run.



Also test with 10 prisoners to verify that the logic is correct for random selection. Random selection should succeed with 10 prisoners at a probability of (1/2)\*\*10, so in 100\_000 simulations, should get pardons about .0977 percent of the time.

```perl
unit sub MAIN (:$prisoners = 100, :$simulations = 10000);
my @prisoners = ^$prisoners;
my $half = floor +@prisoners / 2;

sub random ($n) {
    ^$n .race.map( {
        my @drawers = @prisoners.pick: *;
        @prisoners.map( -> $prisoner {
            my $found = 0;
            for @drawers.pick($half) -> $card {
                $found = 1 and last if $card == $prisoner
            }
            last unless $found;
            $found
        }
        ).sum == @prisoners
    }
    ).grep( *.so ).elems / $n * 100
}

sub optimal ($n) {
    ^$n .race.map( {
        my @drawers = @prisoners.pick: *;
        @prisoners.map( -> $prisoner {
            my $found = 0;
            my $card = @drawers[$prisoner];
            if $card == $prisoner {
                $found = 1
            } else {
                for ^($half - 1) {
                    $card = @drawers[$card];
                    $found = 1 and last if $card == $prisoner
                }
            }
            last unless $found;
            $found
        }
        ).sum == @prisoners
    }
    ).grep( *.so ).elems / $n * 100
}

say "Testing $simulations simulations with $prisoners prisoners.";
printf " Random play wins: %.3f%% of simulations\n", random $simulations;
printf "Optimal play wins: %.3f%% of simulations\n", optimal $simulations;
```


**With defaults**


```
Testing 10000 simulations with 100 prisoners.
 Random play wins: 0.000% of simulations
Optimal play wins: 30.510% of simulations
```


**With passed parameters: --prisoners=10, --simulations=100000**


```
Testing 100000 simulations with 10 prisoners.
 Random play wins: 0.099% of simulations
Optimal play wins: 35.461% of simulations
```
