[1]: https://rosettacode.org/wiki/Word_search

# [Word search][1]

```perl
my $rows = 10;
my $cols = 10;
 
my $message = q:to/END/;
    .....R....
    ......O...
    .......S..
    ........E.
    T........T
    .A........
    ..C.......
    ...O......
    ....D.....
    .....E....
    END
 
my %dir =
    '→' => (1,0),
    '↘' => (1,1),
    '↓' => (0,1),
    '↙' => (-1,1),
    '←' => (-1,0),
    '↖' => (-1,-1),
    '↑' => (0,-1),
    '↗' => (1,-1)
;
 
my @ws = $message.comb(/<print>/);
 
my $path = './unixdict.txt'; # or wherever
 
my @words = $path.IO.slurp.words.grep( { $_ !~~ /<-[a..z]>/ and 2 < .chars < 11 } ).pick(*);
my %index;
my %used;
 
while @ws.first( * eq '.') {
 
    # find an unfilled cell
    my $i = @ws.grep( * eq '.', :k ).pick;
 
    # translate the index to x / y coordinates
    my ($x, $y) = $i % $cols, floor($i / $rows);
 
    # find a word that fits
    my $word = find($x, $y);
 
    # Meh, reached an impasse, easier to just throw it all
    # away and start over rather than trying to backtrack.
    restart, next unless $word;
 
    %used{"$word"}++;
 
    # Keeps trying to place an already used word, choices
    # must be limited, start over
    restart, next if %used{$word} > 15;
 
    # Already used this word, try again
    next if %index{$word.key};
 
    # Add word to used word index
    %index ,= $word;
 
    # place the word into the grid
    place($x, $y, $word);
 
}
 
display();
 
sub display {
    put flat "    ", 'ABCDEFGHIJ'.comb;
    .put for (^10).map: { ($_).fmt("  %2d"), @ws[$_ * $cols .. ($_ + 1) * $cols - 1] }
    put "\n  Words used:";
    my $max = 1 + %index.keys.max( *.chars ).chars;
    for %index.sort {
        printf "%{$max}s %4s %s  ", .key, .value.key, .value.value;
        print "\n" if $++ % 2;
    }
    say "\n"
}
 
sub restart {
    @ws = $message.comb(/<print>/);
    %index = ();
    %used = ();
}
 
sub place ($x is copy, $y is copy, $w) {
    my @word = $w.key.comb;
    my $dir  = %dir{$w.value.value};
    @ws[$y * $rows + $x] = @word.shift;
    while @word {
        ($x, $y) »+=« $dir;
        @ws[$y * $rows + $x] = @word.shift;
    }
 }
 
sub find ($x, $y) {
    my @trials = %dir.keys.map: -> $dir {
            my $space = '.';
            my ($c, $r) = $x, $y;
            loop {
                ($c, $r) »+=« %dir{$dir};
                last if 9 < $r|$c;
                last if 0 > $r|$c;
                my $l = @ws[$r * $rows + $c];
                last if $l ~~ /<:Lu>/;
                $space ~= $l;
            }
            next if $space.chars < 3;
            [$space.trans( '.' => ' ' ),
            ("{'ABCDEFGHIJ'.comb[$x]} {$y}" => $dir)]
        };
 
    for @words.pick(*) -> $word {
        for @trials -> $space {
            next if $word.chars > $space[0].chars;
            return ($word => $space[1]) if compare($space[0].comb, $word.comb)
        }
    }
}
 
sub compare (@s, @w) {
    for ^@w {
        next if @s[$_] eq ' ';
        return False if @s[$_] ne @w[$_]
    }
    True
}
```

#### Output:
```
     A B C D E F G H I J
   0 b y e e a R s w u k
   1 r g e n p f O s e s
   2 d i n l e i i S t i
   3 r e b i l a c e E f
   4 T g t a d a g n l T
   5 d A a t d o w a i d
   6 g i C n a n a l r c
   7 a o g O p a l p r f
   8 p g n p D d a i o a
   9 c r u s h E s p t d

  Words used:
     aaa  G 8 ↖    afield  E 0 ↘  
   alley  F 4 ↖       bye  A 0 →  
 caliber  G 3 ←     crush  A 9 →  
     dan  F 8 ↑       dig  A 5 ↘  
    epic  D 0 ↘       fad  J 7 ↓  
    fisk  J 3 ↑       gap  A 6 ↓  
   geigy  B 4 ↑       get  G 4 ↗  
     gnp  B 8 →       goa  C 7 ←  
    lane  H 6 ↑       law  G 7 ↑  
     nag  D 6 ↖       nne  D 1 ↙  
    odin  F 5 ↖       orr  I 8 ↑  
  paddle  E 7 ↑    picnic  E 1 ↘  
     pip  H 9 ↑       rib  A 1 ↘  
     sir  G 9 ↗       sst  G 0 ↘  
    tail  D 5 ↑       ted  C 4 ↖  
     tor  I 9 ↑      usia  I 0 ↙  
     wei  H 0 ↘  
```
