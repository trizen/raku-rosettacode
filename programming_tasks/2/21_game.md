[1]: https://rosettacode.org/wiki/21_game

# [21 game][1]





Since there is no requirement that the computer play sensibly, it always plays a random guess so the player has some chance to win.

```perl
say qq :to 'HERE';
    The 21 game. Each player chooses to add 1, 2, or 3 to a running total.
    The player whose turn it is when the total reaches 21 wins. Enter q to quit.
    HERE
 
my $total = 0;
loop {
    say "Running total is: $total";
    my ($me,$comp);
    loop {
        $me = prompt 'What number do you play> ';
        last if $me ~~ /^<[123]>$/;
        insult $me;
    }
    $total += $me;
    win('Human') if $total >= 21;
    say "Computer plays: { $comp = (1,2,3).roll }\n";
    $total += $comp;
    win('Computer') if $total >= 21;
}
 
sub win ($player) {
    say "$player wins.";
    exit;
}
 
sub insult ($g) {
    exit if $g eq 'q';
    print ('Yo mama,', 'Jeez,', 'Ummmm,', 'Grow up,', 'Did you even READ the instructions?').roll;
    say " $g is not an integer between 1 & 3..."
}
```

#### Output:
```
The 21 game. Each player chooses to add 1, 2, or 3 to a running total.
The player whose turn it is when the total reaches 21 wins. Enter q to quit.

Running total is: 0
What number do you play> 5
Did you even READ the instructions? 5 is not an integer between 1 & 3...
What number do you play> g
Yo mama, g is not an integer between 1 & 3...
What number do you play> 12
Jeez, 12 is not an integer between 1 & 3...
What number do you play> 3
Computer plays: 2

Running total is: 5
What number do you play> 3
Computer plays: 3

Running total is: 11
What number do you play> 1
Computer plays: 1

Running total is: 13
What number do you play> 3
Computer plays: 2

Running total is: 18
What number do you play> 3
Human wins.
```
