[1]: https://rosettacode.org/wiki/Odd_words

# [Odd words][1]

```perl
my %words = 'unixdict.txt'.IO.slurp.words.map: * => 1;
 
my (@odds, @evens);
 
for %words {
    next if .key.chars < 9;
    my $odd  = .key.comb[0,2 … *].join;
    @odds.push(.key => $odd) if %words{$odd} and $odd.chars > 4;
    my $even = .key.comb[1,3 … *].join;
    @evens.push(.key => $even) if %words{$even} and $even.chars > 4;
}
 
.put for flat 'Odd words > 4:', @odds.sort;
 
.put for flat "\nEven words > 4:", @evens.sort;
```

#### Output:
```
Odd words > 4:
barbarian       brain
childbear       cider
corrigenda      cried
gargantuan      grata
headdress       hades
palladian       plain
propionate      point
salvation       slain
siltation       slain
slingshot       sight
statuette       saute
supersede       spree
supervene       spree
terminable      trial

Even words > 4:
cannonball      annal
importation     motto
psychopomp      scoop
starvation      train
upholstery      posey
```
