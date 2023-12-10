[1]: https://rosettacode.org/wiki/Brilliant_numbers

# [Brilliant numbers][1]

1 through 7 are fast. 8 and 9 take a bit longer.

```perl
use Lingua::EN::Numbers;

# Find an abundance of primes to use to generate brilliants
my %primes = (2..100000).grep( &is-prime ).categorize: { .chars };

# Generate brilliant numbers
my @brilliant = lazy flat (1..*).map: -> $digits {
    sort flat (^%primes{$digits}).race.map: { %primes{$digits}[$_] X× (flat %primes{$digits}[$_ .. *]) }
};

# The task
put "First 100 brilliant numbers:\n" ~ @brilliant[^100].batch(10)».fmt("%4d").join("\n") ~ "\n" ;

for 1 .. 7 -> $oom {
    my $threshold = exp $oom, 10;
    my $key = @brilliant.first: :k, * >= $threshold;
    printf "First >= %13s is %9s in the series: %13s\n", comma($threshold), ordinal-digit(1 + $key, :u), comma @brilliant[$key];
}
```

#### Output:
```
First 100 brilliant numbers:
   4    6    9   10   14   15   21   25   35   49
 121  143  169  187  209  221  247  253  289  299
 319  323  341  361  377  391  403  407  437  451
 473  481  493  517  527  529  533  551  559  583
 589  611  629  649  667  671  689  697  703  713
 731  737  767  779  781  793  799  803  817  841
 851  869  871  893  899  901  913  923  943  949
 961  979  989 1003 1007 1027 1037 1067 1073 1079
1081 1121 1139 1147 1157 1159 1189 1207 1219 1241
1247 1261 1271 1273 1333 1343 1349 1357 1363 1369

First >=            10 is       4ᵗʰ in the series:            10
First >=           100 is      11ᵗʰ in the series:           121
First >=         1,000 is      74ᵗʰ in the series:         1,003
First >=        10,000 is     242ⁿᵈ in the series:        10,201
First >=       100,000 is    2505ᵗʰ in the series:       100,013
First >=     1,000,000 is   10538ᵗʰ in the series:     1,018,081
First >=    10,000,000 is  124364ᵗʰ in the series:    10,000,043
First >=   100,000,000 is  573929ᵗʰ in the series:   100,140,049
First >= 1,000,000,000 is 7407841ˢᵗ in the series: 1,000,000,081
```
