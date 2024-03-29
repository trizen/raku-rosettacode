[1]: https://rosettacode.org/wiki/Guess_the_number/With_feedback

# [Guess the number/With feedback][1]



```perl
my $maxnum = prompt("Hello, please give me an upper boundary: ");
until 0 < $maxnum < Inf {
    say "Oops! The upper boundary should be > 0 and not Inf";
    $maxnum = prompt("Please give me a valid upper boundary: ");
}

my $count = 0;
my $number = (1..$maxnum).pick;

say "I'm thinking of a number from 1 to $maxnum, try to guess it!";
repeat until my $guessed-right {
    given prompt("Your guess: ") {
        when /^[e|q]/ { say 'Goodbye.'; exit; }
        when not 1 <= $_ <= $maxnum {
           say "You really should give me a number from 1 to $maxnum."
        }
        $count++;
        when $number { $guessed-right = True }
        when $number < $_ { say "Sorry, my number is smaller." }
        when $number > $_ { say "Sorry, my number is bigger." }
    }
}
say "Great you guessed right after $count attempts!";
```

#### Output:
```
Hello, please give me an upper boundary: 10 
I'm thinking of a number from 1 to 10, try to guess it!
Your guess: 5
Sorry, my number is bigger.
Your guess: 7
Sorry, my number is smaller.
Your guess: 6
Great you guessed right after 3 attempts!
```
