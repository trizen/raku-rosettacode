[1]: https://rosettacode.org/wiki/Increasing_gaps_between_consecutive_Niven_numbers

# [Increasing gaps between consecutive Niven numbers][1]



```perl
use Lingua::EN::Numbers;

unit sub MAIN (Int $threshold = 10000000);

my int $index = 0;
my int $last  = 0;
my int $gap   = 0;

say 'Gap    Index of gap  Starting Niven';

for 1..* -> \count {
    next unless count %% sum count.comb;
    if (my \diff = count - $last) > $gap {
        $gap = diff;
        printf "%3d %15s %15s\n", $gap, comma($index || 1), comma($last || 1);
    }
    ++$index;
    $last = count;
    last if $index >= $threshold;
}
```

#### Output:
```
Gap    Index of gap  Starting Niven
  1               1               1
  2              10              10
  6              11              12
  7              26              63
  8              28              72
 10              32              90
 12              83             288
 14             102             378
 18             143             558
 23             561           2,889
 32             716           3,784
 36           1,118           6,480
 44           2,948          19,872
 45           4,194          28,971
 54           5,439          38,772
 60          33,494         297,864
 66          51,544         478,764
 72          61,588         589,860
 88          94,748         989,867
 90         265,336       2,879,865
 99         800,054       9,898,956
108       3,750,017      49,989,744
126       6,292,149      88,996,914
```
