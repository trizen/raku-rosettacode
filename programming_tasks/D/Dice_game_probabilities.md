[1]: http://rosettacode.org/wiki/Dice_game_probabilities

# [Dice game probabilities][1]

```perl
sub likelihoods ($roll) {
    my ($dice, $faces) = $roll.comb(/\d+/);
    my @counts;
    @counts[$_]++ for [X+] |((1..$faces,) xx $dice);
    return [@counts[]:p], $faces ** $dice;
}
 
sub beating-probability ([$roll1, $roll2]) {
    my (@c1, $p1) := likelihoods $roll1;
    my (@c2, $p2) := likelihoods $roll2;
    my $p12 = $p1 * $p2;
 
    [+] gather for @c1 X @c2 -> $p, $q {
	take $p.value * $q.value / $p12 if $p.key > $q.key;
    }
}
 
# We're using standard DnD notation for dice rolls here.
say .gist, "\t", .perl given beating-probability < 9d4 6d6 >;
say .gist, "\t", .perl given beating-probability < 5d10 6d7 >;
```

#### Output:
```
0.573144077     <48679795/84934656>
0.64278862872   <3781171969/5882450000>
```


Note that all calculations are in integer and rational arithmetic, so the results in fractional notation are exact.