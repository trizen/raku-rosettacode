[1]: https://rosettacode.org/wiki/Quad-power_prime_seeds

# [Quad-power prime seeds][1]

```perl
use Lingua::EN::Numbers;

my @qpps = lazy (1..*).hyper(:5000batch).grep: -> \n { my \k = n + 1; (n+k).is-prime && (n²+k).is-prime && (n³+k).is-prime && (n⁴+k).is-prime }

say "First fifty quad-power prime seeds:\n" ~ @qpps[^50].batch(10)».&comma».fmt("%7s").join: "\n";

say "\nFirst quad-power prime seed greater than:";

for 1..10 {
    my $threshold = Int(1e6 × $_);
    my $key = @qpps.first: * > $threshold, :k;
    say "{$threshold.&cardinal.fmt: '%13s'} is the {ordinal-digit $key + 1}: {@qpps[$key].&comma}";
}
```

#### Output:
```
First fifty quad-power prime seeds:
      1       2       5       6      69     131     426   1,665   2,129   2,696
  5,250   7,929   9,689  13,545  14,154  14,286  16,434  19,760  25,739  27,809
 29,631  36,821  41,819  46,619  48,321  59,030  60,500  61,955  62,321  73,610
 77,685  79,646  80,535  82,655  85,251  86,996  91,014  96,566  97,739 105,939
108,240 108,681 119,754 122,436 123,164 126,489 140,636 150,480 153,179 163,070

First quad-power prime seed greater than:
  one million is the 141st: 1,009,286
  two million is the 234th: 2,015,496
three million is the 319th: 3,005,316
 four million is the 383rd: 4,004,726
 five million is the 452nd: 5,023,880
  six million is the 514th: 6,000,554
seven million is the 567th: 7,047,129
eight million is the 601st: 8,005,710
 nine million is the 645th: 9,055,151
  ten million is the 701st: 10,023,600
```
