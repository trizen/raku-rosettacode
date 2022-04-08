[1]: https://rosettacode.org/wiki/Round-robin_tournament_schedule

# [Round-robin tournament schedule][1]

```perl
my @players = (1,0)[$_%2] .. $_ given 12;
my $half = +@players div 2;
my $round = 0;
 
loop {
    printf "Round %2d: %s\n", ++$round, "{ zip( @players[^$half], @players[$half..*].reverse ).map: { sprintf "(%2d vs %-2d)", |$_ } }";
    @players[1..*].=rotate(-1);
    last if [<] @players;
}
```

#### Output:
```
Round  1: ( 1 vs 12) ( 2 vs 11) ( 3 vs 10) ( 4 vs 9 ) ( 5 vs 8 ) ( 6 vs 7 )
Round  2: ( 1 vs 11) (12 vs 10) ( 2 vs 9 ) ( 3 vs 8 ) ( 4 vs 7 ) ( 5 vs 6 )
Round  3: ( 1 vs 10) (11 vs 9 ) (12 vs 8 ) ( 2 vs 7 ) ( 3 vs 6 ) ( 4 vs 5 )
Round  4: ( 1 vs 9 ) (10 vs 8 ) (11 vs 7 ) (12 vs 6 ) ( 2 vs 5 ) ( 3 vs 4 )
Round  5: ( 1 vs 8 ) ( 9 vs 7 ) (10 vs 6 ) (11 vs 5 ) (12 vs 4 ) ( 2 vs 3 )
Round  6: ( 1 vs 7 ) ( 8 vs 6 ) ( 9 vs 5 ) (10 vs 4 ) (11 vs 3 ) (12 vs 2 )
Round  7: ( 1 vs 6 ) ( 7 vs 5 ) ( 8 vs 4 ) ( 9 vs 3 ) (10 vs 2 ) (11 vs 12)
Round  8: ( 1 vs 5 ) ( 6 vs 4 ) ( 7 vs 3 ) ( 8 vs 2 ) ( 9 vs 12) (10 vs 11)
Round  9: ( 1 vs 4 ) ( 5 vs 3 ) ( 6 vs 2 ) ( 7 vs 12) ( 8 vs 11) ( 9 vs 10)
Round 10: ( 1 vs 3 ) ( 4 vs 2 ) ( 5 vs 12) ( 6 vs 11) ( 7 vs 10) ( 8 vs 9 )
Round 11: ( 1 vs 2 ) ( 3 vs 12) ( 4 vs 11) ( 5 vs 10) ( 6 vs 9 ) ( 7 vs 8 )
```
