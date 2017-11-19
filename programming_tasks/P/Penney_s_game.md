[1]: http://rosettacode.org/wiki/Penney's_game

# [Penney's game][1]

```perl
enum Coin <Heads Tails>;
enum Yay <Yay Good Super Hah Ooh Yipee Sweet Cool Yes Haha>;
enum Boo <Drat Darn Crumb Oops Rats Bah Criminy Argh Shards>;
enum Bozo «'Dude' 'Cha' 'Bzzt' 'Hey' 'Silly dilly' 'Say what!?' 'You numbskull'»;
 
sub flipping {
    for 1..4 {
        print "-\b";  sleep .1;
        print "\\\b"; sleep .1;
        print "|\b";  sleep .1;
        print "/\b";  sleep .1;
    }
}
 
sub your-choice($p is copy) {
    loop (my @seq; @seq != 3; $p = "{Bozo.pick}! Please pick exactly 3: ") {
        @seq = prompt($p).uc.comb(/ H | T /).map: {
            when 'H' { Heads }
            when 'T' { Tails }
        }
    }
    @seq;
}
 
repeat until prompt("Wanna play again? ").lc ~~ /^n/ {
    my $mefirst = Coin.roll;
    print tc "$mefirst I start, {Coin(+!$mefirst).lc} you start, flipping...\n\t";
    flipping;
    say my $flip = Coin.roll;
 
    my @yours;
    my @mine;
 
    if $flip == $mefirst {
        print "{Yay.pick}! I get to choose first, and I choose: "; sleep 2; say @mine = Coin.roll(3);
        @yours = your-choice("Now you gotta choose: ");
        while @yours eqv @mine {
            say "{Bozo.pick}! We'd both win at the same time if you pick that!";
            @yours = your-choice("Pick something different from me: ");
        }
        say "So, you'll win if we see: ", @yours;
    }
    else {
        @yours = your-choice("{Boo.pick}! First you choose: ");
        say "OK, you'll win if we see: ", @yours;
        print "In that case, I'll just randomly choose: "; sleep 2; say @mine = Coin(+!@yours[1]), @yours[0,1];
    }
 
    sub check($a,$b,$c) {
        given [$a,$b,$c] {
            when @mine  { say "\n{Yay.pick}, I win, and you lose!"; Nil }
            when @yours { say "\n{Boo.pick}, you win, but I'll beat you next time!"; Nil }
            default     { Coin.roll }
        }
    }
 
    sleep 1;
    say < OK! Ready? Right... So... Yo!>.pick;
    sleep .5;
    say ("Pay attention now!",
        "Watch closely!",
        "Let's do it...",
        "You feeling lucky?",
        "No way you gonna win this...",
        "Can I borrow that coin again?").pick;
    sleep 1;
    print "Here we go!\n\t";
    for Coin.roll(3), &check ...^ :!defined {
        flipping;
        print "$_ ";
    }
}
```


Note: the actual run displays a little coin-flipping animation, but that won't show up here:


#### Output:
```
Heads I start, tails you start, flipping...
        Heads
Yipee! I get to choose first, and I choose: Heads Heads Tails
Now you gotta choose: tth
So, you'll win if we see: Tails Tails Heads
Yo!
Can I borrow that coin again?
Here we go!
        Tails Tails Tails Tails Tails Heads 
Argh, you win, but I'll beat you next time!
Wanna play again? y
Tails I start, heads you start, flipping...
        Tails
Yes! I get to choose first, and I choose: Heads Tails Tails
Now you gotta choose: H T T
Dude! We'd both win at the same time if you pick that!
Pick something different from me: heads tails tails
Silly dilly! We'd both win at the same time if you pick that!
Pick something different from me: h,h,h
So, you'll win if we see: Heads Heads Heads
OK!
You feeling lucky?
Here we go!
        Tails Tails Tails Heads Heads Heads 
Drat, you win, but I'll beat you next time!
Wanna play again? y
Heads I start, tails you start, flipping...
        Tails
Shards! First you choose: tht
OK, you'll win if we see: Tails Heads Tails
In that case, I'll just randomly choose: Tails Tails Heads
Right...
Pay attention now!
Here we go!
        Heads Tails Tails Heads 
Hah, I win, and you lose!
Wanna play again? n
```