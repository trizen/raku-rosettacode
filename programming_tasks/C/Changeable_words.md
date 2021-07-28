[1]: https://rosettacode.org/wiki/Changeable_words

# [Changeable words][1]

Sorensen-Dice is very fast to calculate similarities but isn't great for detecting small changes. Levenshtein is great for detecting small changes but isn't very fast.



Get the best of both worlds by doing an initial filter with Sorensen, then get exact results with Levenshtein.

```perl
use Text::Levenshtein;
use Text::Sorensen :sorensen;
 
my @words = grep {.chars > 11}, 'unixdict.txt'.IO.words;
 
my %bi-grams = @words.map: { $_ => .&bi-gram };
 
my %skip = @words.map: { $_ => 0 };
 
say (++$).fmt('%2d'), |$_ for @words.hyper.map: -> $this {
    next if %skip{$this};
    my ($word, @sorensens) = sorensen($this, %bi-grams);
    next unless @sorensens.=grep: { 1 > .[0] > .8 };
    @sorensens = @sorensens»[1].grep: {$this.chars == .chars};
    my @levenshtein = distance($this, @sorensens).grep: * == 1, :k;
    next unless +@levenshtein;
    %skip{$_}++ for @sorensens[@levenshtein];
    ": {$this.fmt('%14s')}  <->  ", @sorensens[@levenshtein].join: ', ';
}
```

#### Output:
```
 1:   aristotelean  <->  aristotelian
 2: claustrophobia  <->  claustrophobic
 3:   committeeman  <->  committeemen
 4: committeeperson  <->  committeepeople
 5:  complimentary  <->  complementary
 6:   confirmation  <->  conformation
 7:  congressperson  <->  congresspeople
 8:   councilperson  <->  councilpeople
 9:   draftsperson  <->  craftsperson
10:   eavesdropped  <->  eavesdropper
11:   frontiersman  <->  frontiersmen
12: handicraftsman  <->  handicraftsmen
13:   incommutable  <->  incomputable
14:   installation  <->  instillation
15:   kaleidescope  <->  kaleidoscope
16:   neuroanatomy  <->  neuroanotomy
17:   newspaperman  <->  newspapermen
18:   nonagenarian  <->  nonogenarian
19:   onomatopoeia  <->  onomatopoeic
20:   philanthrope  <->  philanthropy
21:   proscription  <->  prescription
22:  schizophrenia  <->  schizophrenic
23:  shakespearean  <->  shakespearian
24:   spectroscope  <->  spectroscopy
25:  underclassman  <->  underclassmen
26:  upperclassman  <->  upperclassmen
```
