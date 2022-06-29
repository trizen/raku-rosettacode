[1]: https://rosettacode.org/wiki/Isograms_and_heterograms

# [Isograms and heterograms][1]

```perl
my $file = 'unixdict.txt';

my @words = $file.IO.slurp.words.race.map: { $_ => .comb.Bag };

.say for (6...2).map: -> $n {
    next unless my @iso = @words.race.grep({.value.values.all == $n})».key;
    "\n({+@iso}) {$n}-isograms:\n" ~ @iso.sort({[-.chars, ~$_]}).join: "\n";
}

my $minchars = 10;

say "\n({+$_}) heterograms with more than $minchars characters:\n" ~
  .sort({[-.chars, ~$_]}).join: "\n" given
  @words.race.grep({.key.chars >$minchars && .value.values.max == 1})».key;
```

#### Output:
```
(2) 3-isograms:
aaa
iii

(31) 2-isograms:
beriberi
bilabial
caucasus
couscous
teammate
appall
emmett
hannah
murmur
tartar
testes
anna
coco
dada
deed
dodo
gogo
isis
juju
lulu
mimi
noon
otto
papa
peep
poop
teet
tete
toot
tutu
ii

(32) heterograms with more than 10 characters:
ambidextrous
bluestocking
exclusionary
incomputable
lexicography
loudspeaking
malnourished
atmospheric
blameworthy
centrifugal
christendom
consumptive
countervail
countryside
countrywide
disturbance
documentary
earthmoving
exculpatory
geophysical
inscrutable
misanthrope
problematic
selfadjoint
stenography
sulfonamide
switchblade
switchboard
switzerland
thunderclap
valedictory
voluntarism
```
