[1]: https://rosettacode.org/wiki/Guess_the_number/With_feedback_(player)

# [Guess the number/With feedback (player)][1]



```perl
multi sub MAIN() { MAIN(0, 100) }
multi sub MAIN($min is copy where ($min >= 0), $max is copy where ($max > $min)) {
    say "Think of a number between $min and $max and I'll guess it!";
    while $min <= $max {
        my $guess = (($max + $min)/2).floor;
        given lc prompt "My guess is $guess. Is your number higher, lower or equal? " {
            when /^e/ { say "I knew it!"; exit }
            when /^h/ { $min = $guess + 1      }
            when /^l/ { $max = $guess          }
            default   { say "WHAT!?!?!"        }
        }
    }
    say "How can your number be both higher and lower than $max?!?!?";
}
```


You may execute this program with '`raku program`' or with '`raku program min max`'. Raku creates a usage for us if we don't give the right parameters. It also parses the parameters for us and provides them via `$min` and `$max`. We use multi-subs to provide two MAIN subroutines so the user is able to choose between min and max parameters and no parameters at all, in which case min is set to 0 and max to 100.
