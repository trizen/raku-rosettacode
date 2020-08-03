[1]: https://rosettacode.org/wiki/Word_wheel

# [Word wheel][1]

Everything is adjustable through command line parameters.



Defaults to task specified wheel, unixdict.txt, minimum 3 letters.



Using [Terminal::Boxer](https://modules.raku.org/search/?q=Terminal%3A%3ABoxer) from the Raku ecosystem.

```perl
use Terminal::Boxer;
 
my %*SUB-MAIN-OPTS = :named-anywhere;
 
unit sub MAIN ($wheel = 'ndeokgelw', :$dict = './unixdict.txt', :$min = 3);
 
my $must-have = $wheel.comb[4].lc;
 
my $has = $wheel.comb».lc.Bag;
 
my %words;
$dict.IO.slurp.words».lc.map: {
    next if not .contains($must-have) or .chars < $min;
    %words{.chars}.push: $_ if .comb.Bag ⊆ $has;
};
 
say "Using $dict, minimum $min letters.";
 
print rs-box :3col, :3cw, :indent("\t"), $wheel.comb».uc;
 
say "{sum %words.values».elems} words found";
 
printf "%d letters:  %s\n", .key, .value.sort.join(', ') for %words.sort;
```
```text
raku word-wheel.raku
```

#### Output:
```
Using ./unixdict.txt, minimum 3 letters.
        ╭───┬───┬───╮
        │ N │ D │ E │
        ├───┼───┼───┤
        │ O │ K │ G │
        ├───┼───┼───┤
        │ E │ L │ W │
        ╰───┴───┴───╯
17 words found
3 letters:  eke, elk, keg, ken, wok
4 letters:  keel, keen, keno, knee, knew, know, kong, leek, week, woke
5 letters:  kneel
9 letters:  knowledge
```


Using the much larger dictionary **words.txt** file from **[https://github.com/dwyl/english-words](https://github.com/dwyl/english-words)**

```text
raku word-wheel.raku --dict=./words.txt
```

#### Output:
```
Using ./words.txt, minimum 3 letters.
        ╭───┬───┬───╮
        │ N │ D │ E │
        ├───┼───┼───┤
        │ O │ K │ G │
        ├───┼───┼───┤
        │ E │ L │ W │
        ╰───┴───┴───╯
86 words found
3 letters:  dkg, dkl, eek, egk, eke, ekg, elk, gok, ked, kee, keg, kel, ken, keo, kew, kln, koe, kol, kon, lek, lgk, nek, ngk, oke, owk, wok
4 letters:  deek, deke, doek, doke, donk, eked, elke, elko, geek, genk, gonk, gowk, keel, keen, keld, kele, kend, keno, keon, klee, knee, knew, know, koel, koln, kone, kong, kwon, leek, leke, loke, lonk, okee, oken, week, welk, woke, wolk, wonk
5 letters:  dekle, dekow, gleek, kedge, kendo, kleon, klong, kneed, kneel, knowe, konde, oklee, olnek, woken
6 letters:  gowked, keldon, kelwen, knowle, koleen
8 letters:  weeklong
9 letters:  knowledge
```


Using unixdict.txt:


#### Output:
```
Wheel           words
eimnaprst:      215
celmanort:      215
ceimanrst:      210
elmnaoprt:      208
ahlneorst:      201
```


Using words.txt:


#### Output:
```
Wheel           words
meilanrst:      1329
deilanrst:      1313
ceilanrst:      1301
peilanrst:      1285
geilanrst:      1284
```
