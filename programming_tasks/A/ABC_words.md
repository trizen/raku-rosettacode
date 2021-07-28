[1]: https://rosettacode.org/wiki/ABC_words

# [ABC words][1]

```perl
put display 'unixdict.txt'.IO.words».fc.grep({ (.index('a')//next) < (.index('b')//next) < (.index('c')//next) }),
     :11cols, :fmt('%-12s');
 
sub display ($list, :$cols = 10, :$fmt = '%6d', :$title = "{+$list} matching:\n" )   {
    cache $list;
    $title ~ $list.batch($cols)».fmt($fmt).join: "\n"
}
```

#### Output:
```
55 matching:
aback        abacus       abc          abdicate     abduct       abeyance     abject       abreact      abscess      abscissa     abscissae   
absence      abstract     abstracter   abstractor   adiabatic    aerobacter   aerobic      albacore     alberich     albrecht     algebraic   
alphabetic   ambiance     ambuscade    aminobenzoic anaerobic    arabic       athabascan   auerbach     diabetic     diabolic     drawback    
fabric       fabricate    flashback    halfback     iambic       lampblack    leatherback  metabolic    nabisco      paperback    parabolic   
playback     prefabricate quarterback  razorback    roadblock    sabbatical   snapback     strabismic   syllabic     tabernacle   tablecloth 
```
