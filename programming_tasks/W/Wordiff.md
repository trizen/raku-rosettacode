[1]: https://rosettacode.org/wiki/Wordiff

# [Wordiff][1]

```perl
my @words = 'unixdict.txt'.IO.slurp.lc.words.grep(*.chars > 2);
 
my @small = @words.grep(*.chars < 6);
 
use Text::Levenshtein;
 
my ($rounds, $word, $guess, @used, @possibles) = 0;
 
loop {
    my $lev;
    $word = @small.pick;
    hyper for @words  -> $this {
        next if ($word.chars - $this.chars).abs > 1;
        last if ($lev = distance($word, $this)[0]) == 1;
    }
    last if $lev;
}
 
my $name = ',';
 
#[[### Entirely unnecessary and unuseful "chatty repartee" but is required by the task
 
run 'clear';
$name = prompt "Hello player one, what is your name? ";
say "Cool. I'm going to call you Gomer.";
$name = ' Gomer,';
sleep 1;
say "\nPlayer two, what is your name?\nOh wait, this isn't a \"specified number of players\" game...";
sleep 1;
say "Nevermind.\n";
 
################################################################################]]
 
loop {
    say "Word in play: $word";
    push @used, $word;
    @possibles = @words.hyper.map: -> $this {
        next if ($word.chars - $this.chars).abs > 1;
        $this if distance($word, $this)[0] == 1 and $this ∉ @used;
    }
    $guess = prompt "your word? ";
    last unless $guess ∈ @possibles;
    ++$rounds;
    say qww<Ok! Woot! 'Way to go!' Nice! 👍 😀>.pick ~ "\n";
    $word = $guess;
}
 
my $already = ($guess ∈ @used) ?? " $guess was already played but" !! '';
 
if @possibles {
    say "\nOops. Sorry{$name}{$already} one of [{@possibles}] would have continued the game."
} else {
    say "\nGood job{$name}{$already} there were no possible words to play."
}
say "You made it through $rounds rounds.";
```

#### Output:
```
Hello player one, what is your name? Burtram Redneck
Cool. I'm going to call you Gomer.

Player two, what is your name?
Oh wait, this isn't a "specified number of players" game...
Nevermind.

Word in play: howe
your word? how
Woot!

Word in play: how
your word? show
👍

Word in play: show
your word? shot
Nice!

Word in play: shot
your word? hot
😀

Word in play: hot
your word? hit
Way to go!

Word in play: hit
your word? mit
Nice!

Word in play: mit
your word? kit
😀

Word in play: kit
your word? nit
Woot!

Word in play: nit
your word? nip
😀

Word in play: nip
your word? snip
Ok!

Word in play: snip
your word? slip
Ok!

Word in play: slip
your word? slap
Way to go!

Word in play: slap
your word? lap
Woot!

Word in play: lap
your word? nap
Woot!

Word in play: nap
your word? nan
Nice!

Word in play: nan
your word? man
Nice!

Word in play: man
your word? men
Woot!

Word in play: men
your word? ben
Nice!

Word in play: ben
your word? ban
👍

Word in play: ban
your word? man

Oops. Sorry Gomer, man was already played but one of [bad bag bah bam band bane bang bank bar barn bat bay bean bin bon bran bun can dan fan han ian jan pan ran san tan van wan zan] would have continued the game.
You made it through 19 rounds.
```
