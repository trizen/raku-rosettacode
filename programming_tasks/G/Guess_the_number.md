[1]: http://rosettacode.org/wiki/Guess_the_number

# [Guess the number][1]

```perl6
my $number = (1..10).pick;
repeat {} until prompt("Guess a number: ") == $number;
say "Guessed right!";
```