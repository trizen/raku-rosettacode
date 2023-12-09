[1]: https://rosettacode.org/wiki/Pairs_with_common_factors

# [Pairs with common factors][1]

```perl
use Prime::Factor;
use Lingua::EN::Numbers;

my \𝜑 = 0, |(1..*).hyper.map: -> \t { t × [×] t.&prime-factors.unique.map: { 1 - 1/$_ } }

sub pair-count (\n) {  n × (n - 1) / 2 + 1 - sum 𝜑[1..n] }

say "Number of pairs with common factors - first one hundred terms:\n"
    ~ (1..100).map(&pair-count).batch(10)».&comma».fmt("%6s").join("\n") ~ "\n";

for ^7 { say (my $i = 10 ** $_).&ordinal.tc.fmt("%22s term: ") ~ $i.&pair-count.&comma }
```

#### Output:
```
Number of pairs with common factors - first one hundred terms:
     0      0      0      1      1      4      4      7      9     14
    14     21     21     28     34     41     41     52     52     63
    71     82     82     97    101    114    122    137    137    158
   158    173    185    202    212    235    235    254    268    291
   291    320    320    343    363    386    386    417    423    452
   470    497    497    532    546    577    597    626    626    669
   669    700    726    757    773    818    818    853    877    922
   922    969    969  1,006  1,040  1,079  1,095  1,148  1,148  1,195
 1,221  1,262  1,262  1,321  1,341  1,384  1,414  1,461  1,461  1,526
 1,544  1,591  1,623  1,670  1,692  1,755  1,755  1,810  1,848  1,907

                 First term: 0
                 Tenth term: 14
         One hundredth term: 1,907
        One thousandth term: 195,309
        Ten thousandth term: 19,597,515
One hundred thousandth term: 1,960,299,247
         One millionth term: 196,035,947,609
```
