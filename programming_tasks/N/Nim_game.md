[1]: https://rosettacode.org/wiki/Nim_game

# [Nim game][1]



```perl
say my $tokens = 12, " tokens remaining.\n";

loop {
    my $player = trim prompt "How many tokens do you want to remove; 1, 2 or 3? : ";
    say "Nice try. $tokens tokens remaining.\n" and
    next unless $player eq any <1 2 3>;
    $tokens -= 4;
    say "Computer takes {4 - $player}.\n$tokens tokens remaining.\n";
    say "Computer wins." and last if $tokens <= 0;
}
```

#### Output:
```
12 tokens remaining.

How many tokens do you want to remove; 1, 2 or 3? : 3
Computer takes 1.
8 tokens remaining.

How many tokens do you want to remove; 1, 2 or 3? : 6
Nice try. 8 tokens remaining.

How many tokens do you want to remove; 1, 2 or 3? : G
Nice try. 8 tokens remaining.

How many tokens do you want to remove; 1, 2 or 3? : 2
Computer takes 2.
4 tokens remaining.

How many tokens do you want to remove; 1, 2 or 3? : 1
Computer takes 3.
0 tokens remaining.

Computer wins.
```
