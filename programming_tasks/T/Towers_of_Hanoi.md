[1]: https://rosettacode.org/wiki/Towers_of_Hanoi

# [Towers of Hanoi][1]

```perl
subset Peg of Int where 1|2|3;
 
multi hanoi (0,      Peg $a,     Peg $b,     Peg $c)     { }
multi hanoi (Int $n, Peg $a = 1, Peg $b = 2, Peg $c = 3) {
    hanoi $n - 1, $a, $c, $b;
    say "Move $a to $b.";
    hanoi $n - 1, $c, $b, $a;
}
```