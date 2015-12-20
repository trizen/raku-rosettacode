[1]: http://rosettacode.org/wiki/Monty_Hall_problem

# [Monty Hall problem][1]

This implementation is parametric over the number of doors. [Increasing the number of doors in play makes the superiority of the switch strategy even more obvious](http://en.wikipedia.org/wiki/Monty_Hall_problem#Increasing_the_number_of_doors).

```perl
enum Prize <Car Goat>;
enum Strategy <Stay Switch>;
 
sub play (Strategy $strategy, Int :$doors = 3) returns Prize {
 
    # Call the door with a car behind it door 0. Number the
    # remaining doors starting from 1.
    my Prize @doors = flat Car, Goat xx $doors - 1;
 
    # The player chooses a door.
    my Prize $initial_pick = @doors.splice(@doors.keys.pick,1)[0];
 
    # Of the n doors remaining, the host chooses n - 1 that have
    # goats behind them and opens them, removing them from play.
    while @doors > 1 {
	@doors.splice($_,1)
	    when Goat
		given @doors.keys.pick;
    }
 
    # If the player stays, they get their initial pick. Otherwise,
    # they get whatever's behind the remaining door.
    return $strategy === Stay ?? $initial_pick !! @doors[0];
}
 
constant TRIALS = 1000;
 
for 3, 10 -> $doors {
    my %wins;
    say "With $doors doors: ";
    for Stay, 'Staying', Switch, 'Switching' -> $s, $name {
        for ^TRIALS {
            ++%wins{$s} if play($s, doors => $doors) == Car;
        }
        say "  $name wins ",
            round(100*%wins{$s} / TRIALS),
            '% of the time.'
    }
}
```

#### Output:
```
With 3 doors: 
  Staying wins 31% of the time.
  Switching wins 68% of the time.
With 10 doors: 
  Staying wins 9% of the time.
  Switching wins 90% of the time.
```