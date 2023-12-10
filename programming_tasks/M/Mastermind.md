[1]: https://rosettacode.org/wiki/Mastermind

# [Mastermind][1]





By default, plays classic Mastermind using letters in place of colors. ( 4 chosen from 6, no repeats, 10 guess limit. ) Pass in parameters to modify the game. Enter a string of --length (default 4) letters with or without spaces. Guesses accept lower or upper case.

```perl
sub MAIN (
    Int :$colors  where 1 < * < 21 = 6,  Int :$length  where 3 < * < 11 = 4,
    Int :$guesses where 7 < * < 21 = 10, Bool :$repeat = False
  ) {
    my @valid = ('A' .. 'T')[^$colors];
    my $puzzle = $repeat ?? @valid.roll($length) !! @valid.pick($length);
    my @guesses;

    my $black = '●';
    my $white = '○';

    loop {
        clearscr();
        say header();
        printf " %{$length * 2}s :: %s\n", @guesses[$_][0], @guesses[$_][1] for ^@guesses;
        say '';
        lose() if @guesses == $guesses;
        my $guess = get-guess();
        next unless $guess.&is-valid;
        my $score = score($puzzle, $guess);
        win() if $score eq ($black xx $length).join: ' ';
        @guesses.push: [$guess, $score];
    }

    sub header {
        my $num = $guesses - @guesses;
        qq:to/END/;
        Valid letter, but wrong position: ○ - Correct letter and position: ●
        Guess the {$length} element sequence containing the letters {@valid}
        Repeats are {$repeat ?? '' !! 'not '}allowed. You have $num guess{ $num == 1 ?? '' !! 'es'} remaining.
        END
    }

    sub score ($puzzle, $guess) {
        my @score;
        for ^$length {
            if $puzzle[$_] eq $guess[$_] {
                @score.push: $black;
            }
            elsif $puzzle[$_] eq any(@$guess) {
                @score.push: $white;
            }
            else {
                @score.push('-');
            }
        }
        @score.sort.reverse.join: ' ';
    }

    sub clearscr { $*KERNEL ~~ /'win32'/ ?? run('cls') !! run('clear') }

    sub get-guess { (uc prompt 'Your guess?: ').comb(/@valid/) }

    sub is-valid (@guess) { so $length == @guess }

    sub win  { say 'You Win! The correct answer is: ', $puzzle; exit }

    sub lose { say 'Too bad, you ran out of guesses. The solution was: ', $puzzle; exit }
}
```

#### Output:
```
Valid letter, but wrong position: ○ - Correct letter and position: ●
Guess the 4 element sequence containing the letters A B C D E F
Repeats are not allowed. You have 5 guesses remaining.

  A B C D :: ○ ○ ○ -
  C A B E :: ● ○ ○ -
  D A E F :: ● ○ - -
  B A E C :: ● ○ ○ -
  D E B C :: ○ ○ ○ ○

Your guess?: cdeb
You Win! The correct answer is: (C D E B)
```
