[1]: https://rosettacode.org/wiki/Poker_hand_analyser

# [Poker hand analyser][1]

This solution handles jokers. It has been written as a Perl 6 grammar.

```perl
use v6;
 
grammar PokerHand {
 
    # Perl6 Grammar to parse and rank 5-card poker hands
    # E.g. PokerHand.parse("2♥ 3♥ 2♦ 3♣ 3♦");
    # 2013-12-21: handle 'joker' wildcards; maximum of two
 
    rule TOP {
        <hand>
 
        :my ($n, $flush, $straight);
        {
            $n        = n-of-a-kind($<hand>);
            $flush    = flush($<hand>);
            $straight = straight($<hand>);
         }
         <rank($n[0], $n[1], $flush, $straight)>
     }
 
    rule hand {
         :my %*PLAYED;
         { %*PLAYED = () }
         [ <face-card> | <joker> ]**5
    }
 
    token face-card {<face><suit> <?{
            my $card = ~$/.lc;
            # disallow duplicates
            ++%*PLAYED{$card} <= 1;
       }>
    }
 
    token joker {:i 'joker' <?{
        my $card = ~$/.lc;
            # allow two jokers in a hand
            ++%*PLAYED{$card} <= 2;
        }>
    }
 
    token face {:i <[2..9 jqka]> | 10 }
    token suit {<[♥ ♦ ♣ ♠]>}
 
    multi token rank(5,$,$,$)                   { $<five-of-a-kind>=<?> }
    multi token rank($,$f,$s  where {$f && $s}) { $<straight-flush>=<?> }
    multi token rank(4,$,$,$)                   { $<four-of-a-kind>=<?> }
    multi token rank($,$,$f,$ where {$f})       { $<flush>=<?> }
    multi token rank($,$,$,$s where {$s})       { $<straight>=<?> }
    multi token rank(3,2,$,$)                   { $<full-house>=<?> }
    multi token rank(3,$,$,$)                   { $<three-of-a-kind>=<?> }
    multi token rank(2,2,$,$)                   { $<two-pair>=<?> }
    multi token rank(2,$,$,$)                   { $<one-pair>=<?> }
    multi token rank($,$,$,$) is default        { $<high-card>=<?> }
 
   sub n-of-a-kind($/) {
        my %faces := bag @<face-card>.map( -> $/ {~$<face>.lc} );
        my @counts = %faces.values.sort.reverse;
        @counts[0] += @<joker>;
       return @counts;
    }
 
    sub flush($/) {
        my %suits := set @<face-card>.map( -> $/ {~$<suit>} );
        return +%suits == 1;
    }
 
   sub straight($/) {
        # allow both ace-low and ace-high straights
        constant @Seq = < a 2 3 4 5 6 7 8 9 10 j q k a >.map: *.Str;
        my $faces = set @<face-card>.map( -> $/ {~$<face>.lc} );
        my $jokers = +@<joker>;
 
        for 0..(+@Seq - 5) {
 
            my $run = set @Seq[$_ .. $_+4];
 
            return True
                if +($faces ∩ $run) == 5 - $jokers;
        }
        return False;
    }
}
 
for ("2♥ 2♦ 2♣ k♣ q♦",   # three-of-a-kind
     "2♥ 5♥ 7♦ 8♣ 9♠",   # high-card
     "a♥ 2♦ 3♣ 4♣ 5♦",   # straight
     "2♥ 3♥ 2♦ 3♣ 3♦",   # full-house
     "2♥ 7♥ 2♦ 3♣ 3♦",   # two-pair
     "2♥ 7♥ 7♦ 7♣ 7♠",   # four-of-a-kind
     "10♥ j♥ q♥ k♥ a♥",  # straight-flush
     "4♥ 4♠ k♠ 5♦ 10♠",  # one-pair
     "q♣ 10♣ 7♣ 6♣ 4♣",  # flush
     ## EXTRA CREDIT ##
     "joker  2♦  2♠  k♠  q♦",  # three-of-a-kind
     "joker  5♥  7♦  8♠  9♦",  # straight
     "joker  2♦  3♠  4♠  5♠",  # straight
     "joker  3♥  2♦  3♠  3♦",  # four-of-a-kind
     "joker  7♥  2♦  3♠  3♦",  # three-of-a-kind
     "joker  7♥  7♦  7♠  7♣",  # five-of-a-kind
     "joker  j♥  q♥  k♥  A♥",  # straight-flush
     "joker  4♣  k♣  5♦ 10♠",  # one-pair
     "joker  k♣  7♣  6♣  4♣",  # flush
     "joker  2♦ joker  4♠  5♠",  # straight
     "joker  Q♦ joker  A♠ 10♠",  # straight
     "joker  Q♦ joker  A♦ 10♦",  # straight-flush
     "joker  2♦ 2♠  joker  q♦",  # four of a kind
   ) {
   PokerHand.parse($_);
   my $rank = $<rank>
      ?? $<rank>.caps
      !! 'invalid';
   say "$_: $rank";
}
```

#### Output:
```
   2♥ 2♦ 2♣ k♣ q♦: three-of-a-kind
   2♥ 5♥ 7♦ 8♣ 9♠: high-card
   a♥ 2♦ 3♣ 4♣ 5♦: straight
   2♥ 3♥ 2♦ 3♣ 3♦: full-house
   2♥ 7♥ 2♦ 3♣ 3♦: two-pair
   2♥ 7♥ 7♦ 7♣ 7♠: four-of-a-kind
   10♥ j♥ q♥ k♥ a♥: straight-flush
   4♥ 4♠ k♠ 5♦ 10♠: one-pair
   q♣ 10♣ 7♣ 6♣ 4♣: flush
   joker  2♦  2♠  k♠  q♦: three-of-a-kind
   joker  5♥  7♦  8♠  9♦: straight
   joker  2♦  3♠  4♠  5♠: straight
   joker  3♥  2♦  3♠  3♦: four-of-a-kind
   joker  7♥  2♦  3♠  3♦: three-of-a-kind
   joker  7♥  7♦  7♠  7♣: five-of-a-kind
   joker  j♥  q♥  k♥  A♥: straight-flush
   joker  4♣  k♣  5♦ 10♠: one-pair
   joker  k♣  7♣  6♣  4♣: flush
   joker  2♦  joker  4♠  5♠: straight
   joker  Q♦  joker  A♠ 10♠: straight
   joker  Q♦  joker  A♦ 10♦: straight-flush
   joker  2♦  2♠  joker  q♦: four-of-a-kind
```