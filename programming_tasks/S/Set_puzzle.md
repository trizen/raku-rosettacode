[1]: http://rosettacode.org/wiki/Set_puzzle

# [Set puzzle][1]

The trick here is to allocate three different bits for each enum, with the result that the cards of a matching set OR together to produce a 4-digit octal number that contains only the digits 1, 2, 4, or 7. This OR is done by funny looking `[+|]`, which is the reduction form of `+|`, which is the numeric bitwise OR. (Because Perl 6 stole the bare `|` operator for composing junctions instead.)

```perl
enum Color (red => 0o1000, green =>  0o2000, purple => 0o4000);
enum Count (one =>  0o100, two =>     0o200, three =>   0o400);
enum Shape (oval =>  0o10, squiggle => 0o20, diamond =>  0o40);
enum Style (solid =>  0o1, open =>      0o2, striped =>   0o4);
 
my @deck := (Color.enums X Count.enums X Shape.enums X Style.enums).tree;
 
sub MAIN($DRAW = 9, $GOAL = $DRAW div 2) {
    sub show-cards(@c) { printf "    %-6s %-5s %-8s %s\n", $_».key for @c }
 
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
    red    two   diamond  striped
    purple one   squiggle solid
    purple three squiggle solid
    red    two   squiggle striped
    red    two   oval     striped
    green  one   diamond  open
    red    three diamond  solid
    green  three squiggle open
    purple two   diamond  striped

Set 1:
    red    two   diamond  striped
    red    two   squiggle striped
    red    two   oval     striped

Set 2:
    purple one   squiggle solid
    red    two   squiggle striped
    green  three squiggle open

Set 3:
    purple three squiggle solid
    red    two   oval     striped
    green  one   diamond  open

Set 4:
    green  one   diamond  open
    red    three diamond  solid
    purple two   diamond  striped
```