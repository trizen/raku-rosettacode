[1]: http://rosettacode.org/wiki/Number_reversal_game

# [Number reversal game][1]

Do-at-least-once loops are fairly rare, but this program wants to have two of them. We use the built-in <tt>.pick(\*)</tt> method to shuffle the numbers. We use <tt>.=</tt> to dispatch a mutating method in two spots; the first is just a different way to write <tt>++</tt>, while the second of these reverses an array slice in place. The <tt>[&lt;]</tt> is a reduction operator on less than, so it returns true if the elements of the list are strictly ordered. We also see in the first repeat loop that, although the while condition is not tested till after the loop, the while condition can in fact declare the variable that will be initialized the first time through the loop, which is a neat trick, and not half unreadable once you get used to it.

```perl6
repeat while [<] my @jumbled-list {
    @jumbled-list = (1..9).pick(*)
}
 
my $turn = 0;
repeat until [<] @jumbled-list {
    my $d = prompt $turn.=succ.fmt('%2d: ') ~
                   @jumbled-list ~
                   " - Flip how many digits? "
        or exit;
    @jumbled-list[^$d] .= reverse;
}
 
say "    @jumbled-list[]";
say "You won in $turn turns.";
```


Output:


#### Output:
```
 1: 3 5 8 2 7 9 6 1 4 - Flip how many digits ? 6
 2: 9 7 2 8 5 3 6 1 4 - Flip how many digits ? 9
 3: 4 1 6 3 5 8 2 7 9 - Flip how many digits ? 6
 4: 8 5 3 6 1 4 2 7 9 - Flip how many digits ? 8
 5: 7 2 4 1 6 3 5 8 9 - Flip how many digits ? 7
 6: 5 3 6 1 4 2 7 8 9 - Flip how many digits ? 3
 7: 6 3 5 1 4 2 7 8 9 - Flip how many digits ? 6
 8: 2 4 1 5 3 6 7 8 9 - Flip how many digits ? 4
 9: 5 1 4 2 3 6 7 8 9 - Flip how many digits ? 5
10: 3 2 4 1 5 6 7 8 9 - Flip how many digits ? 3
11: 4 2 3 1 5 6 7 8 9 - Flip how many digits ? 4
12: 1 3 2 4 5 6 7 8 9 - Flip how many digits ? 2
13: 3 1 2 4 5 6 7 8 9 - Flip how many digits ? 3
14: 2 1 3 4 5 6 7 8 9 - Flip how many digits ? 2
    1 2 3 4 5 6 7 8 9
You won in 14 turns.
```