[1]: http://rosettacode.org/wiki/Ranking_methods

# [Ranking methods][1]

```perl
my @scores =
    Solomon => 44,
    Jason   => 42,
    Errol   => 42,
    Garry   => 41,
    Bernard => 41,
    Barry   => 41,
    Stephen => 39;
 
sub tiers (@s) { @s.classify(*.value).pairs.sort.reverse.map: { [.value».key] } }
 
sub standard (@s) {
    my $rank = 1;
    gather for tiers @s -> @players {
	take $rank => @players;
	$rank += @players;
    }
}
 
sub modified (@s) {
    my $rank = 0;
    gather for tiers @s -> @players {
	$rank += @players;
	take $rank => @players;
    }
}
 
sub dense (@s) { tiers(@s).map: { ++$_ => @^players } }
 
sub ordinal (@s) { @s.map: ++$_ => *.key }
 
sub fractional (@s) {
    my $rank = 1;
    gather for tiers @s -> @players {
	my $beg = $rank;
	my $end = $rank += @players;
	take [+]($beg ..^ $end) / @players => @players;
    }
}
 
say   "Standard:";   .say for   standard @scores;
say "\nModified:";   .say for   modified @scores;
say "\nDense:";      .say for      dense @scores;
say "\nOrdinal:";    .say for    ordinal @scores;
say "\nFractional:"; .say for fractional @scores;
```

#### Output:
```
Standard:
1 => ["Solomon"]
2 => ["Jason", "Errol"]
4 => ["Garry", "Bernard", "Barry"]
7 => ["Stephen"]

Modified:
1 => ["Solomon"]
3 => ["Jason", "Errol"]
6 => ["Garry", "Bernard", "Barry"]
7 => ["Stephen"]

Dense:
1 => ["Solomon"]
2 => ["Jason", "Errol"]
3 => ["Garry", "Bernard", "Barry"]
4 => ["Stephen"]

Ordinal:
1 => "Solomon"
2 => "Jason"
3 => "Errol"
4 => "Garry"
5 => "Bernard"
6 => "Barry"
7 => "Stephen"

Fractional:
1.0 => ["Solomon"]
2.5 => ["Jason", "Errol"]
5.0 => ["Garry", "Bernard", "Barry"]
7.0 => ["Stephen"]
```