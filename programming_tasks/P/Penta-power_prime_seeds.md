[1]: https://rosettacode.org/wiki/Penta-power_prime_seeds

# [Penta-power prime seeds][1]

```perl
use Lingua::EN::Numbers;

my @ppps = lazy (^∞).hyper(:5000batch).map(* × 2 + 1).grep: -> \n { my \k = n + 1; (1+k).is-prime && (n+k).is-prime && (n²+k).is-prime && (n³+k).is-prime && (n⁴+k).is-prime }

say "First thirty penta-power prime seeds:\n" ~ @ppps[^30].batch(10)».&comma».fmt("%9s").join: "\n";

say "\nFirst penta-power prime seed greater than:";

for 1..10 {
    my $threshold = Int(1e6 × $_);
    my $key = @ppps.first: * > $threshold, :k;
    say "{$threshold.&cardinal.fmt: '%13s'} is the {ordinal-digit $key + 1}: {@ppps[$key].&comma}";
}
```

#### Output:
```
First thirty penta-power prime seeds:
        1         5        69     1,665     2,129    25,739    29,631    62,321    77,685    80,535
   82,655   126,489   207,285   211,091   234,359   256,719   366,675   407,945   414,099   628,859
  644,399   770,531   781,109   782,781   923,405 1,121,189 1,158,975 1,483,691 1,490,475 1,512,321

First penta-power prime seed greater than:
  one million is the 26th: 1,121,189
  two million is the 39th: 2,066,079
three million is the 47th: 3,127,011
 four million is the 51st: 4,059,525
 five million is the 59th: 5,279,175
  six million is the 63rd: 6,320,601
seven million is the 68th: 7,291,361
eight million is the 69th: 8,334,915
 nine million is the 71st: 9,100,671
  ten million is the 72nd: 10,347,035
```
