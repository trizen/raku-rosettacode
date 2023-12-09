[1]: https://rosettacode.org/wiki/Upside-down_numbers

# [Upside-down numbers][1]

```perl
use Lingua::EN::Numbers;

sub udgen (@r) {
    my @u = @r.hyper.map: { next if .contains: 0; ($_, (10 «-« .flip.comb).join) };
    @u».join, @u».join(5)
}

my @upside-downs = lazy flat 5, (^∞).map({ udgen exp($_,10) .. exp(1+$_,10) });

say "First fifty upside-downs:\n" ~ @upside-downs[^50].batch(10)».fmt("%4d").join: "\n";

say '';

for 5e2, 5e3, 5e4, 5e5, 5e6 {
    say "{.Int.&ordinal.tc}: " ~ comma @upside-downs[$_-1]
}
```

#### Output:
```
First fifty upside-downs:
   5   19   28   37   46   55   64   73   82   91
 159  258  357  456  555  654  753  852  951 1199
1289 1379 1469 1559 1649 1739 1829 1919 2198 2288
2378 2468 2558 2648 2738 2828 2918 3197 3287 3377
3467 3557 3647 3737 3827 3917 4196 4286 4376 4466

Five hundredth: 494,616
Five thousandth: 56,546,545
Fifty thousandth: 6,441,469,664
Five hundred thousandth: 729,664,644,183
Five millionth: 82,485,246,852,682
```
