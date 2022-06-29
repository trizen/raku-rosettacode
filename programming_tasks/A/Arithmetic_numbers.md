[1]: https://rosettacode.org/wiki/Arithmetic_numbers

# [Arithmetic numbers][1]

```perl
use Prime::Factor;
use Lingua::EN::Numbers;

my @arithmetic = lazy (1..∞).hyper.grep: { my @div = .&divisors; (@div.sum / +@div).narrow ~~ Int }

say "The first { .Int.&cardinal } arithmetic numbers:\n", @arithmetic[^$_].batch(10)».fmt("%{.chars}d").join: "\n" given 1e2;

for 1e3, 1e4, 1e5, 1e6 {
    say "\nThe { .Int.&ordinal }: { comma @arithmetic[$_-1] }";
    say "Composite arithmetic numbers ≤ { comma @arithmetic[$_-1] }: { comma +@arithmetic[^$_].grep({!.is-prime}) - 1 }";
}
```

#### Output:
```
The first one hundred arithmetic numbers:
  1   3   5   6   7  11  13  14  15  17
 19  20  21  22  23  27  29  30  31  33
 35  37  38  39  41  42  43  44  45  46
 47  49  51  53  54  55  56  57  59  60
 61  62  65  66  67  68  69  70  71  73
 77  78  79  83  85  86  87  89  91  92
 93  94  95  96  97  99 101 102 103 105
107 109 110 111 113 114 115 116 118 119
123 125 126 127 129 131 132 133 134 135
137 138 139 140 141 142 143 145 147 149

The one thousandth: 1,361
Composite arithmetic numbers ≤ 1,361: 782

The ten thousandth: 12,953
Composite arithmetic numbers ≤ 12,953: 8,458

The one hundred thousandth: 125,587
Composite arithmetic numbers ≤ 125,587: 88,219

The one millionth: 1,228,663
Composite arithmetic numbers ≤ 1,228,663: 905,043
```
