[1]: https://rosettacode.org/wiki/Generate_Chess960_starting_position

# [Generate Chess960 starting position][1]


First, using a list with three rooks and no king, we keep generating a random piece order until the two bishops are on opposite colors.  Then we sneakily promote the second of the three rooks to a king.

```perl
repeat until m/ '♗' [..]* '♗' / { $_ = < ♖ ♖ ♖ ♕ ♗ ♗ ♘ ♘ >.pick(*).join }
s:2nd['♖'] = '♔';
say .comb;
```

#### Output:
```
♕ ♗ ♖ ♘ ♔ ♖ ♗ ♘
```


Here's a more "functional" solution that avoids side effects

```perl
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
but written in the form of a list comprehension rather than nested map and grep.  (The list comprehension is actually faster currently.)  Note that the constant is calculated at compile time, because, well, it's a constant.  Just a big fancy one.

```perl
constant chess960 =
   < ♛ ♜ ♜ ♜ ♝ ♝ ♞ ♞ >.permutations».join.unique.grep( / '♝' [..]* '♝' / )».subst(:nth(2), /'♜'/, '♚');

.say for chess960;
```


Here's a much faster way (about 30x) to generate all 960 variants by construction.  No need to filter for uniqueness, since it produces exactly 960 entries.

```perl
constant chess960 = gather for 0..3 -> $q {
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


If you run this you'll see that most of the time is spent in compilation, so in the case of separate precompilation the table of 960 entries merely needs to be deserialized back into memory.  Picking from those entries guarantees uniform distribution over all possible boards.


```
♛♝♜♚♝♞♞♜
```


### Raku: Generate from SP-ID



There is a [standard numbering scheme](https://en.wikipedia.org/wiki/Fischer_random_chess_numbering_scheme) for Chess960 positions, assigning each an index in the range 0..959. This function will generate the corresponding position from a given index number (or fall back to a random one if no index is specified, making it yet another solution to the general problem).

```perl
subset Pos960 of Int where { $_ ~~ ^960 };
sub chess960(Pos960 $position = (^960).pick) {

  # We remember the remainder used to place first bishop in order to place the
  # second
  my $b1;
  
  # And likewise remember the chosen combination for knights between those
  # placements
  my @n;
  
  # Piece symbols and positioning rules in order.  Start with the position
  # number. At each step, divide by the divisor; the quotient becomes the
  # dividend for the next step.  Feed the remainder into the specified code block
  # to get a space number N, then place the piece in the Nth empty space left in
  # the array.
  my @rules = (
    #divisor, mapping function,                      piece
    ( 4,      { $b1 = $_; 2 * $_ + 1 },              '♝'  ),
    ( 4,      { 2 * $_ - ($_ > $b1 ?? 1 !! 0) },     '♝'  ),
    ( 6,      { $_ },                                '♛'  ),
    (10,      { @n = combinations(5,2)[$_]; @n[0] }, '♞'  ),
    ( 1,      { @n[1]-1 },                           '♞'  ),
    ( 1,      { 0 },                                 '♜'  ),
    ( 1,      { 0 },                                 '♚'  ),
    ( 1,      { 0 },                                 '♜'  )
  
  );
  
  # Initial array, using '-' to represent empty spaces
  my @array = «-» xx 8;
  
  # Working value that starts as the position number but is divided by the
  # divisor at each placement step.
  my $p = $position;
  
  # Loop over the placement rules
  for @rules -> ($divisor, $block, $piece) {
  
    # get remainder when divided by divisor
    (my $remainder, $p) = $p.polymod($divisor);
  
    # apply mapping function
    my $space = $block($remainder);
  
    # find index of the $space'th element of the array that's still empty
    my $index = @array.kv.grep(-> $i,$v { $v eq '-' })[$space][0];
  
    # and place the piece
    @array[$index] = $piece;
  }
  return @array;
}

# demo code
say chess960(518); #standard optning position
say chess960; # (it happened to pick #300)
```

#### Output:
```
♜ ♞ ♝ ♛ ♚ ♝ ♞ ♜
♛ ♝ ♞ ♜ ♚ ♜ ♝ ♞
```
