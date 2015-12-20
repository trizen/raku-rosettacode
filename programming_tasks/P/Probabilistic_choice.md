[1]: http://rosettacode.org/wiki/Probabilistic_choice

# [Probabilistic choice][1]

```perl
constant TRIALS = 1e6;
 
constant @event = <aleph beth gimel daleth he waw zayin heth>;
 
constant @P = flat (1 X/ 5 .. 11), 1759/27720;
constant @cP = [\+] @P;
 
my @results;
@results[ @cP.first: { $_ > once rand }, :k ]++ xx TRIALS;
 
say  'Event    Occurred Expected  Difference';
for ^@results {
    my ($occurred, $expected) = @results[$_], @P[$_] * TRIALS;
    printf "%-9s%8.0f%9.1f%12.1f\n",
            @event[$_],
                $occurred,
                     $expected,
                          abs $occurred - $expected;
}
```

#### Output:
```
Event    Occurred Expected  Difference
aleph      200369 200000.0       369.0
beth       167005 166666.7       338.3
gimel      142690 142857.1       167.1
daleth     125061 125000.0        61.0
he         110563 111111.1       548.1
waw        100214 100000.0       214.0
zayin       90617  90909.1       292.1
heth        63481  63456.0        25.0
```