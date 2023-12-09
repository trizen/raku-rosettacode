[1]: https://rosettacode.org/wiki/Pan_base_non-primes

# [Pan base non-primes][1]

```perl
use Base::Any;
use List::Divvy;

my @np = 4,6,8,9, |lazy (11..*).hyper.grep( -> $n { ($n.substr(*-1) eq '0') || (1 < [gcd] $n.comb».Int) || none (2..$n).map: { try "$n".&from-base($_).is-prime } } );

put "First 50 pan-base composites:\n" ~ @np[^50].batch(10)».fmt("%3s").join: "\n";
put "\nFirst 20 odd pan-base composites:\n" ~ @np.grep(* % 2)[^20].batch(10)».fmt("%3s").join: "\n";

my $threshold = 2500;
put "\nCount of pan-base composites up to and including $threshold: " ~ +@np.&upto($threshold);

put "Percent odd  up to and including $threshold: " ~ +@np.&upto($threshold).grep(* % 2) / +@np.&upto($threshold) × 100;
put "Percent even up to and including $threshold: " ~ +@np.&upto($threshold).grep(* %% 2) / +@np.&upto($threshold) × 100;
```

#### Output:
```
First 50 pan-base composites:
  4   6   8   9  20  22  24  26  28  30
 33  36  39  40  42  44  46  48  50  55
 60  62  63  64  66  68  69  70  77  80
 82  84  86  88  90  93  96  99 100 110
112 114 116 118 120 121 130 132 134 136

First 20 odd pan-base composites:
  9  33  39  55  63  69  77  93  99 121
143 165 169 187 231 253 273 275 297 299

Count of pan-base composites up to and including 2500: 953
Percent odd  up to and including 2500: 16.894019
Percent even up to and including 2500: 83.105981
```
