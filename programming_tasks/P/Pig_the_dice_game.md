[1]: https://rosettacode.org/wiki/Pig_the_dice_game

# [Pig the dice game][1]



```perl
constant DIE = 1..6;
 
sub MAIN (Int :$players = 2, Int :$goal = 100) {
    my @safe = 0 xx $players;
    for |^$players xx * -> $player {
	say "\nOK, player #$player is up now.";
	my $safe = @safe[$player];
	my $ante = 0;
	until $safe + $ante >= $goal or
	    prompt("#$player, you have $safe + $ante = {$safe+$ante}. Roll? [Yn] ") ~~ /:i ^n/
	{
	    given DIE.roll {
		say "  You rolled a $_.";
		when 1 {
		    say "  Bust!  You lose $ante but keep your previous $safe.";
		    $ante = 0;
		    last;
		}
		when 2..* {
		    $ante += $_;
		}
	    }
	}
	$safe += $ante;
	if $safe >= $goal {
	    say "\nPlayer #$player wins with a score of $safe!";
	    last;
	}
	@safe[$player] = $safe;
	say "  Sticking with $safe." if $ante;
    }
}
```


The game defaults to the specified task, but we'll play a shorter game with three players for our example:


```
> pig help
Usage:
  pig [--players=<Int>] [--goal=<Int>]
> pig --players=3 --goal=20

OK, player #0 is up now.
#0, you have 0 + 0 = 0. Roll? [Yn] 
  You rolled a 6.
#0, you have 0 + 6 = 6. Roll? [Yn] 
  You rolled a 6.
#0, you have 0 + 12 = 12. Roll? [Yn] n
  Sticking with 12.

OK, player #1 is up now.
#1, you have 0 + 0 = 0. Roll? [Yn] 
  You rolled a 4.
#1, you have 0 + 4 = 4. Roll? [Yn] 
  You rolled a 6.
#1, you have 0 + 10 = 10. Roll? [Yn] 
  You rolled a 6.
#1, you have 0 + 16 = 16. Roll? [Yn] n
  Sticking with 16.

OK, player #2 is up now.
#2, you have 0 + 0 = 0. Roll? [Yn] 
  You rolled a 5.
#2, you have 0 + 5 = 5. Roll? [Yn] 
  You rolled a 1.
  Bust!  You lose 5 but keep your previous 0.

OK, player #0 is up now.
#0, you have 12 + 0 = 12. Roll? [Yn] 
  You rolled a 1.
  Bust!  You lose 0 but keep your previous 12.

OK, player #1 is up now.
#1, you have 16 + 0 = 16. Roll? [Yn] n

OK, player #2 is up now.
#2, you have 0 + 0 = 0. Roll? [Yn] 
  You rolled a 6.
#2, you have 0 + 6 = 6. Roll? [Yn] 
  You rolled a 6.
#2, you have 0 + 12 = 12. Roll? [Yn] 
  You rolled a 4.
#2, you have 0 + 16 = 16. Roll? [Yn] 
  You rolled a 6.

Player #2 wins with a score of 22!
```
