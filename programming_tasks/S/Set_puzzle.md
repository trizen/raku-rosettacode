[1]: https://rosettacode.org/wiki/Set_puzzle

# [Set puzzle][1]

The trick here is to allocate three different bits for each enum, with the result that the cards of a matching set OR together to produce a 4-digit octal number that contains only the digits 1, 2, 4, or 7. This OR is done by funny looking `[+|]`, which is the reduction form of `+|`, which is the numeric bitwise OR. (Because Perl 6 stole the bare `|` operator for composing junctions instead.)

```raku
enum Color (red => 0o1000, green =>  0o2000, purple => 0o4000);
enum Count (one =>  0o100, two =>     0o200, three =>   0o400);
enum Shape (oval =>  0o10, squiggle => 0o20, diamond =>  0o40);
enum Style (solid =>  0o1, open =>      0o2, striped =>   0o4);
 
my @deck = Color.enums X Count.enums X Shape.enums X Style.enums;
 
sub MAIN($DRAW = 9, $GOAL = $DRAW div 2) {
    sub show-cards(@c) { { printf "%9s%7s%10s%9s\n", @c[$_;*]».key } for ^@c }
 
    my @combinations = [^$DRAW].combinations(3);
 
    my @draw;
    repeat until (my @sets) == $GOAL {
        @draw = @deck.pick($DRAW);
        my @bits = @draw.map: { [+] @^enums».value }
        @sets = gather for @combinations -> @c {
            take @draw[@c].item when /^ <[1247]>+ $/ given ( [+|] @bits[@c] ).base(8);
        }
    }
 
    say "Drew $DRAW cards:";
    show-cards @draw;
    for @sets.kv -> $i, @cards {
        say "\nSet {$i+1}:";
        show-cards @cards;
    }
}
```

#### Output:
```
Drew 9 cards:
   purple    two   diamond     open
      red    two  squiggle  striped
   purple  three  squiggle     open
   purple    two  squiggle  striped
      red  three      oval  striped
      red    one   diamond  striped
   purple    two      oval    solid
    green  three   diamond    solid
      red    two  squiggle     open

Set 1:
   purple    two   diamond     open
   purple    two  squiggle  striped
   purple    two      oval    solid

Set 2:
   purple    two   diamond     open
      red    one   diamond  striped
    green  three   diamond    solid

Set 3:
      red    two  squiggle  striped
      red  three      oval  striped
      red    one   diamond  striped

Set 4:
   purple  three  squiggle     open
      red  three      oval  striped
    green  three   diamond    solid
```