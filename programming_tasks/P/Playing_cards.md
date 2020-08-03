[1]: https://rosettacode.org/wiki/Playing_cards

# [Playing cards][1]

```raku
enum Pip <A 2 3 4 5 6 7 8 9 10 J Q K>;
enum Suit <♦ ♣ ♥ ♠>;
 
class Card {
    has Pip $.pip;
    has Suit $.suit;
 
    method Str { $!pip ~ $!suit }
}
 
class Deck {
    has Card @.cards = pick *,
            map { Card.new(:$^pip, :$^suit) }, flat (Pip.pick(*) X Suit.pick(*));
 
    method shuffle { @!cards .= pick: * }
 
    method deal { shift @!cards }
 
    method Str  { ~@!cards }
    method gist { ~@!cards }
}
 
my Deck $d = Deck.new;
say "Deck: $d";
 
my $top = $d.deal;
say "Top card: $top";
 
$d.shuffle;
say "Deck, re-shuffled: ", $d;
```

#### Output:
```
Deck: 3♦ J♦ 4♥ 7♠ 7♣ 7♥ 9♣ K♥ 6♠ 2♦ 3♠ Q♥ 8♥ 2♥ J♥ 5♥ 8♦ 8♣ 6♦ 7♦ 5♦ 2♣ 4♦ 8♠ 9♥ 4♣ 3♥ K♠ 2♠ 5♣ Q♣ Q♦ K♦ 4♠ 9♦ Q♠ 5♠ 6♥ J♣ J♠ K♣ 9♠ 3♣ 6♣
Top card: 3♦
Deck, re-shuffled: K♦ 4♣ J♠ 2♥ J♥ K♣ 6♣ 5♠ 3♥ 6♦ 5♦ 4♠ J♣ 4♦ 6♥ K♥ 7♥ 7♦ 2♦ 4♥ 6♠ 7♣ 9♦ 3♣ 3♠ 2♣ 2♠ 8♦ 5♣ 9♠ 5♥ J♦ 9♥ Q♦ Q♣ Q♥ Q♠ 8♥ 8♠ K♠ 9♣ 8♣ 7♠
```