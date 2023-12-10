[1]: https://rosettacode.org/wiki/Harmonic_series

# [Harmonic series][1]

Using [Lingua::EN::Numbers](https://modules.raku.org/search/?q=Lingua%3A%3AEN%3A%3ANumbers) from the [Raku ecosystem](https://modules.raku.org/).

```perl
use Lingua::EN::Numbers;

my @H = [\+] (1..*).map: { FatRat.new: 1, $_ };

say "First twenty harmonic numbers as rationals:\n",
    @H[^20]».&pretty-rat.batch(5)».fmt("%18s").join: "\n";

put "\nOne Hundredth:\n", pretty-rat @H[99];

say "\n(zero based) Index of first value:";
printf "  greater than %2d: %6s (%s term)\n",
  $_, comma( my $i = @H.first(* > $_, :k) ), ordinal 1 + $i for 1..10;
```

#### Output:
```
First twenty harmonic numbers as rationals:
                 1                3/2               11/6              25/12             137/60
             49/20            363/140            761/280          7129/2520          7381/2520
       83711/27720        86021/27720     1145993/360360     1171733/360360     1195757/360360
    2436559/720720  42142223/12252240   14274301/4084080 275295799/77597520  55835135/15519504

One Hundredth:
14466636279520351160221518043104131447711/2788815009188499086581352357412492142272

(zero based) Index of first value:
  greater than  1:      1 (second term)
  greater than  2:      3 (fourth term)
  greater than  3:     10 (eleventh term)
  greater than  4:     30 (thirty-first term)
  greater than  5:     82 (eighty-third term)
  greater than  6:    226 (two hundred twenty-seventh term)
  greater than  7:    615 (six hundred sixteenth term)
  greater than  8:  1,673 (one thousand, six hundred seventy-fourth term)
  greater than  9:  4,549 (four thousand, five hundred fiftieth term)
  greater than 10: 12,366 (twelve thousand, three hundred sixty-seventh term)
```
