[1]: https://rosettacode.org/wiki/Solve_the_no_connection_puzzle

# [Solve the no connection puzzle][1]

This uses a Warnsdorff solver, which cuts down the number of tries by more than a factor of six over the brute force approach. This same solver is used in:



The idiosyncratic adjacency diagram is dealt with by the simple expedient of bending the two vertical lines `||` into two bows `)(`, such that adjacency can be calculated simply as a distance of 2 or less.

```raku
my @adjacent = gather -> $y, $x {
    take [$y,$x] if abs($x|$y) > 2;
} for flat -5 .. 5 X -5 .. 5;
 
solveboard q:to/END/;
    . _ . . _ .
    . . . . . .
    _ . _ 1 . _
    . . . . . .
    . _ . . _ .
    END
 
sub solveboard($board) {
    my $max = +$board.comb(/\w+/);
    my $width = $max.chars;
 
    my @grid;
    my @known;
    my @neigh;
    my @degree;
 
    @grid = $board.lines.map: -> $line {
        [ $line.words.map: { /^_/ ?? 0 !! /^\./ ?? Rat !! $_ } ]
    }
 
    sub neighbors($y,$x --> List) {
        eager gather for @adjacent {
            my $y1 = $y + .[0];
            my $x1 = $x + .[1];
            take [$y1,$x1] if defined @grid[$y1][$x1];
        }
    }
 
    for ^@grid -> $y {
        for ^@grid[$y] -> $x {
            if @grid[$y][$x] -> $v {
                @known[$v] = [$y,$x];
            }
            if @grid[$y][$x].defined {
                @neigh[$y][$x] = neighbors($y,$x);
                @degree[$y][$x] = +@neigh[$y][$x];
            }
        }
    }
    print "\e[0H\e[0J";
 
    my $tries = 0;
 
    try_fill 1, @known[1];
 
    sub try_fill($v, $coord [$y,$x] --> Bool) {
        return True if $v > $max;
        $tries++;
 
        my $old = @grid[$y][$x];
 
        return False if +$old and $old != $v;
        return False if @known[$v] and @known[$v] !eqv $coord;
 
        @grid[$y][$x] = $v;               # conjecture grid value
 
        print "\e[0H";                    # show conjectured board
        for @grid -> $r {
            say do for @$r {
                when Rat { ' ' x $width }
                when 0   { '_' x $width }
                default  { .fmt("%{$width}d") }
            }
        }
 
 
        my @neighbors = @neigh[$y][$x][];
 
        my @degrees;
        for @neighbors -> \n [$yy,$xx] {
            my $d = --@degree[$yy][$xx];  # conjecture new degrees
            push @degrees[$d], n;         # and categorize by degree
        }
 
        for @degrees.grep(*.defined) -> @ties {
            for @ties.reverse {           # reverse works better for this hidato anyway
                return True if try_fill $v + 1, $_;
            }
        }
 
        for @neighbors -> [$yy,$xx] {
            ++@degree[$yy][$xx];          # undo degree conjectures
        }
 
        @grid[$y][$x] = $old;             # undo grid value conjecture
        return False;
    }
 
    say "$tries tries";
}
```

#### Output:
```
  4     3  
           
2   8 1   7
           
  6     5  
18 tries
```