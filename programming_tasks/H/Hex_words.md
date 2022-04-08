[1]: https://rosettacode.org/wiki/Hex_words

# [Hex words][1]

Sorted by digital root with a secondary alphabetical sort.

```perl
sub dr (Int $_ is copy) { $_ = dr(.comb.sum) while .chars > 1; $_ }
 
my %hex = './unixdict.txt'.IO.slurp.words.grep( *.chars > 3 )\
  .grep({ not / <-[abcdef]> / }).map: { $_ => dr :16($_).comb.sum }
 
say "{+%hex} hex words longer than 3 characters found in unixdict.txt:";
printf "%6s ➡ %8d ➡ %d\n", .key, :16(.key), .value for %hex.sort: { .value, .key }
 
my %many = %hex.grep: +*.key.comb.Set > 3;
 
say "\nOf which {+%many} contain at least four distinct characters:";
printf "%6s ➡ %8d ➡ %d\n", .key, :16(.key), .value for %many.sort: { -:16(.key) }
```

#### Output:
```
26 hex words longer than 3 characters found in unixdict.txt:
 ababa ➡   703162 ➡ 1
  abbe ➡    43966 ➡ 1
  dada ➡    56026 ➡ 1
  deaf ➡    57007 ➡ 1
decade ➡ 14600926 ➡ 1
  cede ➡    52958 ➡ 2
  feed ➡    65261 ➡ 2
  abed ➡    44013 ➡ 3
 added ➡   712173 ➡ 3
  bade ➡    47838 ➡ 3
 beebe ➡   782014 ➡ 4
 decca ➡   912586 ➡ 4
  dade ➡    56030 ➡ 5
  bead ➡    48813 ➡ 6
deface ➡ 14613198 ➡ 6
  babe ➡    47806 ➡ 7
  fade ➡    64222 ➡ 7
  dead ➡    57005 ➡ 8
efface ➡ 15727310 ➡ 8
facade ➡ 16435934 ➡ 8
accede ➡ 11325150 ➡ 9
  beef ➡    48879 ➡ 9
  cafe ➡    51966 ➡ 9
 dacca ➡   896202 ➡ 9
  deed ➡    57069 ➡ 9
  face ➡    64206 ➡ 9

Of which 13 contain at least four distinct characters:
facade ➡ 16435934 ➡ 8
efface ➡ 15727310 ➡ 8
deface ➡ 14613198 ➡ 6
decade ➡ 14600926 ➡ 1
accede ➡ 11325150 ➡ 9
 decca ➡   912586 ➡ 4
  fade ➡    64222 ➡ 7
  face ➡    64206 ➡ 9
  deaf ➡    57007 ➡ 1
  cafe ➡    51966 ➡ 9
  bead ➡    48813 ➡ 6
  bade ➡    47838 ➡ 3
  abed ➡    44013 ➡ 3
```
