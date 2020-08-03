[1]: https://rosettacode.org/wiki/Fairshare_between_two_and_more

# [Fairshare between two and more][1]

Add an extension showing the relative fairness correlation of this selection algorithm. An absolutely fair algorithm would have a correlation of 1 for each person (no person has an advantage or disadvantage due to an algorithmic artefact.) This algorithm is fair, and gets better the more iterations are done.



A lower correlation factor corresponds to an advantage, higher to a disadvantage, the closer to 1 it is, the fairer the algorithm. Absolute best possible advantage correlation is 0. Absolute worst is 2.

```raku
sub fairshare (\b) { ^∞ .hyper.map: { .polymod( b xx * ).sum % b } }
 
.say for <2 3 5 11>.map: { .fmt('%2d:') ~ .&fairshare[^25]».fmt('%2d').join: ', ' }
 
say "\nRelative fairness of this method. Scaled fairness correlation. The closer to 1.0 each person
is, the more fair the selection algorithm is. Gets better with more iterations.";
 
for <2 3 5 11 39> -> $people {
    print "\n$people people: \n";
    for $people * 1, $people * 10, $people * 1000 -> $iterations {
        my @fairness;
        fairshare($people)[^$iterations].kv.map: { @fairness[$^v % $people] += $^k }
        my $scale = @fairness.sum / @fairness;
        my @range = @fairness.map( { $_ / $scale } );
        printf "After round %4d: Best advantage: %-10.8g - Worst disadvantage: %-10.8g - Spread between best and worst: %-10.8g\n",
            $iterations/$people, @range.min, @range.max, @range.max - @range.min;
    }
}
```

#### Output:
```
 2: 0,  1,  1,  0,  1,  0,  0,  1,  1,  0,  0,  1,  0,  1,  1,  0,  1,  0,  0,  1,  0,  1,  1,  0,  0
 3: 0,  1,  2,  1,  2,  0,  2,  0,  1,  1,  2,  0,  2,  0,  1,  0,  1,  2,  2,  0,  1,  0,  1,  2,  1
 5: 0,  1,  2,  3,  4,  1,  2,  3,  4,  0,  2,  3,  4,  0,  1,  3,  4,  0,  1,  2,  4,  0,  1,  2,  3
11: 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10,  0,  2,  3,  4

Relative fairness of this method. Scaled fairness correlation. The closer to 1.0 each person
is, the more fair the selection algorithm is. Gets better with more iterations.

2 people: 
After round    1: Best advantage: 0          - Worst disadvantage: 2          - Spread between best and worst: 2         
After round   10: Best advantage: 1          - Worst disadvantage: 1          - Spread between best and worst: 0         
After round 1000: Best advantage: 1          - Worst disadvantage: 1          - Spread between best and worst: 0         

3 people: 
After round    1: Best advantage: 0          - Worst disadvantage: 2          - Spread between best and worst: 2         
After round   10: Best advantage: 0.99310345 - Worst disadvantage: 1.0068966  - Spread between best and worst: 0.013793103
After round 1000: Best advantage: 0.99999933 - Worst disadvantage: 1.0000007  - Spread between best and worst: 1.3337779e-06

5 people: 
After round    1: Best advantage: 0          - Worst disadvantage: 2          - Spread between best and worst: 2         
After round   10: Best advantage: 1          - Worst disadvantage: 1          - Spread between best and worst: 0         
After round 1000: Best advantage: 1          - Worst disadvantage: 1          - Spread between best and worst: 0         

11 people: 
After round    1: Best advantage: 0          - Worst disadvantage: 2          - Spread between best and worst: 2         
After round   10: Best advantage: 0.99082569 - Worst disadvantage: 1.0091743  - Spread between best and worst: 0.018348624
After round 1000: Best advantage: 0.99999909 - Worst disadvantage: 1.0000009  - Spread between best and worst: 1.8183471e-06

39 people: 
After round    1: Best advantage: 0          - Worst disadvantage: 2          - Spread between best and worst: 2         
After round   10: Best advantage: 0.92544987 - Worst disadvantage: 1.0745501  - Spread between best and worst: 0.14910026
After round 1000: Best advantage: 0.99999103 - Worst disadvantage: 1.000009   - Spread between best and worst: 1.7949178e-05
```
