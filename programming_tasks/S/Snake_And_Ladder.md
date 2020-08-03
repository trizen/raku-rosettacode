[1]: https://rosettacode.org/wiki/Snake_And_Ladder

# [Snake And Ladder][1]

Snakes and ladders is entirely chance based, so human interaction is not really required. This version allows up to one human player against any number of computer players and asks for input... but doesn't really *need* or even *use* it. I didn't bother to implement a graphical interface.

```raku
        # board layout
my %snl =  4, 14,  9, 31, 17,  7, 20, 38, 28, 84, 40, 59, 51, 67, 54, 34,
          62, 19, 63, 81, 64, 60, 71, 91, 87, 24, 93, 73, 95, 75, 99, 78;
 
my @players = 1, 1, 1; # three players, starting on square 1
my $human = 1;         # player 1 is human. set to 0 for all computer players
 
loop {
    for ^@players -> $player {
        turn(@players[$player], $player + 1);
    }
    say '';
}
 
sub turn ($square is rw, $player) {
    if $player == $human {
        prompt "You are on square $square. Hit enter to roll the die.";
    }
    my $roll = (1..6).roll;
    my $turn = $square + $roll;
    printf "Player $player on square %2d rolls a $roll", $square;
    if $turn > 100 {
        say " but cannot move. Next players turn.";
        return $square;
    }
    if %snl{$turn} {
        $square = %snl{$turn};
        if $turn > $square {
            say ". Oops! Landed on a snake. Slither down to $square."
        } else {
            say ". Yay! Landed on a ladder. Climb up to $square."
        }
    } else {
        $square = $turn;
        say " and moves to square $square";
    }
    say "Player $player wins!" and exit if $square == 100;
    return $square;
}
```

#### Output:
```
You are on square 1. Hit enter to roll the die.
Player 1 on square  1 rolls a 1 and moves to square 2
Player 2 on square  1 rolls a 4 and moves to square 5
Player 3 on square  1 rolls a 1 and moves to square 2

You are on square 2. Hit enter to roll the die.
Player 1 on square  2 rolls a 1 and moves to square 3
Player 2 on square  5 rolls a 2 and moves to square 7
Player 3 on square  2 rolls a 3 and moves to square 5

You are on square 3. Hit enter to roll the die.
Player 1 on square  3 rolls a 4 and moves to square 7
Player 2 on square  7 rolls a 1 and moves to square 8
Player 3 on square  5 rolls a 2 and moves to square 7

You are on square 7. Hit enter to roll the die.
Player 1 on square  7 rolls a 2. Yay! Landed on a ladder. Climb up to 31.
Player 2 on square  8 rolls a 3 and moves to square 11
Player 3 on square  7 rolls a 2. Yay! Landed on a ladder. Climb up to 31.

  ... about 15 turns omitted ...

You are on square 90. Hit enter to roll the die.
Player 1 on square 90 rolls a 3. Oops! Landed on a snake. Slither down to 73.
Player 2 on square 55 rolls a 3 and moves to square 58
Player 3 on square 90 rolls a 4 and moves to square 94

You are on square 73. Hit enter to roll the die.
Player 1 on square 73 rolls a 6 and moves to square 79
Player 2 on square 58 rolls a 5. Yay! Landed on a ladder. Climb up to 81.
Player 3 on square 94 rolls a 3 and moves to square 97

You are on square 79. Hit enter to roll the die.
Player 1 on square 79 rolls a 3 and moves to square 82
Player 2 on square 81 rolls a 6. Oops! Landed on a snake. Slither down to 24.
Player 3 on square 97 rolls a 5 but cannot move. Next players turn.

You are on square 82. Hit enter to roll the die.
Player 1 on square 82 rolls a 5. Oops! Landed on a snake. Slither down to 24.
Player 2 on square 24 rolls a 6 and moves to square 30
Player 3 on square 97 rolls a 4 but cannot move. Next players turn.

You are on square 24. Hit enter to roll the die.
Player 1 on square 24 rolls a 4. Yay! Landed on a ladder. Climb up to 84.
Player 2 on square 30 rolls a 5 and moves to square 35
Player 3 on square 97 rolls a 4 but cannot move. Next players turn.

You are on square 84. Hit enter to roll the die.
Player 1 on square 84 rolls a 1 and moves to square 85
Player 2 on square 35 rolls a 2 and moves to square 37
Player 3 on square 97 rolls a 3 and moves to square 100
Player 3 wins!
```