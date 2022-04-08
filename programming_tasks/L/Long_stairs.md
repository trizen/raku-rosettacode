[1]: https://rosettacode.org/wiki/Long_stairs

# [Long stairs][1]

```perl
my ($trials, $t-total, $s-total) = 10000;
 
say 'Seconds  steps behind  steps ahead';
 
race for ^$trials {
    my $stairs   = 100;
    my $location = 0;
    my $seconds  = 0;
 
    loop {
        ++$seconds;
        ++$location;
        last if $location > $stairs;
        for (1..$stairs).roll(5) {
            ++$location if $_ <= $location;
            ++$stairs;
        }
        say "  $seconds        $location         {$stairs-$location}" if !$_ && (599 < $seconds < 610);
    }
 
    $t-total += $seconds;
    $s-total += $stairs;
}
 
say "Average seconds: {$t-total/$trials},  Average steps: {$s-total/$trials}";
```

#### Output:
```
Seconds  steps behind  steps ahead
  600        2143         957
  601        2149         956
  602        2153         957
  603        2158         957
  604        2164         956
  605        2170         955
  606        2175         955
  607        2178         957
  608        2183         957
  609        2187         958
Average seconds: 2716.0197,  Average steps: 13677.143
```
