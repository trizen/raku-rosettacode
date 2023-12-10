[1]: https://rosettacode.org/wiki/Anadromes

# [Anadromes][1]

```perl
my @words = 'words.txt'.IO.slurp.words.grep: *.chars > 6;

my %words = @words.pairs.invert;

put join "\n", @words.map: { %words{$_}:delete and sprintf "%10s ↔ %s", $_, .flip if ($_ ne .flip) && %words{.flip} }
```

#### Output:
```
   amaroid ↔ diorama
   degener ↔ reneged
   deifier ↔ reified
   deliver ↔ reviled
   dessert ↔ tressed
  desserts ↔ stressed
   deviler ↔ relived
  dioramas ↔ samaroid
   gateman ↔ nametag
   leveler ↔ relevel
   pat-pat ↔ tap-tap
  redrawer ↔ rewarder
   reknits ↔ stinker
   relever ↔ reveler
   reliver ↔ reviler
   revotes ↔ setover
   sallets ↔ stellas
```
