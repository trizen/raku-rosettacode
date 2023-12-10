[1]: https://rosettacode.org/wiki/Pig_the_dice_game/Player

# [Pig the dice game/Player][1]





This implements a pig player class where you can customize the strategy it uses. Pass a strategy code reference in that will evaluate to a Boolean value. The player will roll the die then decide whether to roll again or lock in its winnings based on its strategy. It will continue to roll until it gets a 1 (bust) or the strategy code reference evaluates to True (finished turn).



Set up as many players as you want, then run it. It will play 100 games (by default) then report the score for each game then the win totals for each player. If you want to play a different number of games, pass the number at the command line as a parameter.



Here we have 5 players:


```
 player 0 uses the default strategy, always roll if it can. 
 player 1 will roll up to 5 times then lock in whatever it earned.
 player 2 will try to get at least 20 points per turn.
 player 3 randomly picks whether to roll again or not biased so that there is a 90% chance that it will.
 player 4 randomly chooses to roll again but gets more consrvative as its score get closer to the goal.
```
```perl
my $games = @*ARGS ?? (shift @*ARGS) !! 100;

constant DIE = 1 .. 6;
constant GOAL = 100;

class player {
    has $.score    is rw = 0;
    has $.ante     is rw;
    has $.rolls    is rw;
    has &.strategy is rw = sub { False }; # default, always roll again

    method turn {
        my $done_turn = False;
        $.rolls = 0;
        $.ante  = 0;
        repeat {
            given DIE.roll {
                $.rolls++;
                when 1 {
                    $.ante = 0;
                    $done_turn = True;
                }
                when 2..* {
                    $.ante += $_;
                }
            }
            $done_turn = True if $.score + $.ante >= GOAL or (&.strategy)();
        } until $done_turn;
        $.score += $.ante;
    }
}

my @players;

# default, go-for-broke, always roll again
@players[0] = player.new;

# try to roll 5 times but no more per turn
@players[1] = player.new( strategy => sub { @players[1].rolls >= 5 } );

# try to accumulate at least 20 points per turn
@players[2] = player.new( strategy => sub { @players[2].ante > 20 } );

# random but 90% chance of rolling again
@players[3] = player.new( strategy => sub { 1.rand < .1 } );

# random but more conservative as approaches goal
@players[4] = player.new( strategy => sub { 1.rand < ( GOAL - @players[4].score ) * .6 / GOAL } );

my @wins = 0 xx @players;

for ^ $games {
    my $player = -1;
    repeat {
        $player++;
        @players[$player % @players].turn;
    } until @players[$player % @players].score >= GOAL;

    @wins[$player % @players]++;

    say join "\t", @players>>.score;
    @players[$_].score = 0 for ^@players; # reset scores for next game
}

say "\nSCORES: for $games games";
say join "\t", @wins;
```


**Sample output for 10000 games**


```
0       103     46      5       40
0       100     69      0       48
0       105     22      7       44
0       75      69      19      102
105     21      23      5       17
0       101     85      12      29
0       70      66      103     23
0       0       104     69      20
0       100     44      0       20
0       102     63      75      30
0       56      101     12      40
0       103     71      2       38
0       103     91      21      32
0       18      102     47      28
...
...
...
104     0       69      14      47
0       68      101     13      22
0       99      89      102     31

SCORES: for 10000 games
947     3534    3396    1714    409
```
