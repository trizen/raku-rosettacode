[1]: https://rosettacode.org/wiki/Sleeping_Beauty_problem

# [Sleeping Beauty problem][1]

```perl
sub sleeping-beauty ($trials) {
    my $gotheadsonwaking = 0;
    my $wakenings = 0;
    ^$trials .map: {
        given <Heads Tails>.roll {
            ++$wakenings;
            when 'Heads' { ++$gotheadsonwaking }
            when 'Tails' { ++$wakenings }
        }
    }
    say "Wakenings over $trials experiments: ", $wakenings;
    $gotheadsonwaking / $wakenings
}
 
say "Results of experiment:  Sleeping Beauty should estimate a credence of: ", sleeping-beauty(1_000_000);
```

#### Output:
```
Wakenings over 1000000 experiments: 1500040
Results of experiment:  Sleeping Beauty should estimate a credence of: 0.333298
```
