[1]: https://rosettacode.org/wiki/Set,_the_card_game

# [Set, the card game][1]

```perl
my @attributes = <one two three>, <solid striped open>, <red green purple>, <diamond oval squiggle>;

sub face ($_) { .polymod(3 xx 3).kv.map({ @attributes[$^k;$^v] }) ~ ('s' if $_%3) }

sub sets (@cards) { @cards.combinations(3).race.grep: { !(sum ([Z+] $_».polymod(3 xx 3)) »%» 3) } }

for 4,8,12 -> $deal {
    my @cards = (^81).pick($deal);
    my @sets = @cards.&sets;
    say "\nCards dealt: $deal";
    for @cards { put .&face };
    say "\nSets found: {+@sets}";
    for @sets { put .map(&face).join("\n"), "\n" };
}

say "\nIn the whole deck, there are {+(^81).&sets} sets.";
```

#### Output:
```
Cards dealt: 4
one open purple squiggle
one striped red squiggle
three striped green diamonds
one open green diamond

Sets found: 0

Cards dealt: 8
three striped purple squiggles
three open green diamonds
one striped purple oval
three open red squiggles
two striped red diamonds
one solid purple diamond
one solid red oval
one solid green diamond

Sets found: 2
three open green diamonds
two striped red diamonds
one solid purple diamond

three open red squiggles
two striped red diamonds
one solid red oval

Cards dealt: 12
three open purple squiggles
one striped purple diamond
two striped red squiggles
two striped green squiggles
one solid green oval
three open red squiggles
two striped purple diamonds
three striped purple squiggles
one open red diamond
two striped red diamonds
two striped green ovals
one open green oval

Sets found: 3
three open purple squiggles
one solid green oval
two striped red diamonds

two striped red squiggles
two striped purple diamonds
two striped green ovals

one solid green oval
three open red squiggles
two striped purple diamonds


In the whole deck, there are 1080 sets.
```
