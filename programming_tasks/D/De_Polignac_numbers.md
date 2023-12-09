[1]: https://rosettacode.org/wiki/De_Polignac_numbers

# [De Polignac numbers][1]

```perl
use List::Divvy;
use Lingua::EN::Numbers;
constant @po2 = (1..∞).map: 2 ** *;
my @dePolignac = lazy 1, |(2..∞).hyper.map(* × 2 + 1).grep: -> $n { all @po2.&upto($n).map: { !is-prime $n - $_ } };

say "First fifty de Polignac numbers:\n" ~ @dePolignac[^50]».&comma».fmt("%5s").batch(10).join: "\n";
say "\nOne thousandth: " ~ comma @dePolignac[999];
say "\nTen thousandth: " ~ comma @dePolignac[9999];
```

#### Output:
```
First fifty de Polignac numbers:
    1   127   149   251   331   337   373   509   599   701
  757   809   877   905   907   959   977   997 1,019 1,087
1,199 1,207 1,211 1,243 1,259 1,271 1,477 1,529 1,541 1,549
1,589 1,597 1,619 1,649 1,657 1,719 1,759 1,777 1,783 1,807
1,829 1,859 1,867 1,927 1,969 1,973 1,985 2,171 2,203 2,213

One thousandth: 31,941

Ten thousandth: 273,421
```
