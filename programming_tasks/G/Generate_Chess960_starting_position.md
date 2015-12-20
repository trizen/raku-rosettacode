[1]: http://rosettacode.org/wiki/Generate_Chess960_starting_position

# [Generate Chess960 starting position][1]

First, using a list with three rooks and no king, we keep generating a random piece order until the two bishops are on opposite colors. Then we sneakily promote the second of the three rooks to a king.

```perl6
repeat until m/ '♗' [..]* '♗' / { $_ = < ♖ ♖ ♖ ♕ ♗ ♗ ♘ ♘ >.pick(*).join }
s:2nd['♖'] = '♔';
say .comb;
```

#### Output:
```
♕ ♗ ♖ ♘ ♔ ♖ ♗ ♘
```


Here's a more "functional" solution that avoids side effects

```perl6
sub chess960 {
    .subst(:nth(2), /'♜'/, '♚') given
    first rx/ '♝' [..]* '♝' /,
    < ♛ ♜ ♜ ♜ ♝ ♝ ♞ ♞ >.pick(*).join xx *;
}
 
say chess960;
```

#### Output:
```
♛♝♜♚♝♞♞♜
```


We can also pregenerate the list of 960 positions, though the method we use below is a bit wasteful, since it
generates 40320 candidates only to throw most of them away. This is essentially the same filtering algorithm
but written in the form of a list comprehension rather than nested map and grep. (The list comprehension is actually faster currently.) Note that the constant is calculated at compile time, because, well, it's a constant. Just a big fancy one.

```perl6
constant chess960 = eager
    .subst(:nth(2), /'♜'/, '♚') 
        if / '♝' [..]* '♝' /
            for < ♛ ♜ ♜ ♜ ♝ ♝ ♞ ♞ >.permutations».join.uniq;
 
.say for chess960;
```


Here's a much faster way (about 30x) to generate all 960 variants by construction. No need to filter for uniqueness, since it produces exactly 960 entries.

```perl6
constant chess960 = eager gather for 0..3 -> $q {
    (my @q = <♜ ♚ ♜>).splice($q, 0, '♛');
    for 0 .. @q -> $n1 {
        (my @n1 = @q).splice($n1, 0, '♞');
        for $n1 ^.. @n1 -> $n2 {
            (my @n2 = @n1).splice($n2, 0, '♞');
            for 0 .. @n2 -> $b1 {
                (my @b1 = @n2).splice($b1, 0, '♝');
                for $b1+1, $b1+3 ...^ * > @b1 -> $b2 {
                    (my @b2 = @b1).splice($b2, 0, '♝');
                    take @b2.join;
                }
            }
        }
    }
}
 
CHECK { note "done compiling" }
note +chess960;
say chess960.pick;
```

#### Output:
```
done compiling
960
♜♚♝♜♞♛♞♝
```


If you run this you'll see that most of the time is spent in compilation, so in the case of separate precompilation the table of 960 entries merely needs to be deserialized back into memory. Picking from those entries guarantees uniform distribution over all possible boards.