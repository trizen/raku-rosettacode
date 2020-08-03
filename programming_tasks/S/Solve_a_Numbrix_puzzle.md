[1]: https://rosettacode.org/wiki/Solve_a_Numbrix_puzzle

# [Solve a Numbrix puzzle][1]

This uses a Warnsdorff solver, which cuts down the number of tries by more than a factor of six over the brute force approach. This same solver is used in:

```raku
my @adjacent =           [-1, 0],
               [ 0, -1],          [ 0, 1],
                         [ 1, 0];
put "\n" xx 60;
 
solveboard q:to/END/;
    __ __ __ __ __ __ __ __ __
    __ __ 46 45 __ 55 74 __ __
    __ 38 __ __ 43 __ __ 78 __
    __ 35 __ __ __ __ __ 71 __
    __ __ 33 __ __ __ 59 __ __
    __ 17 __ __ __ __ __ 67 __
    __ 18 __ __ 11 __ __ 64 __
    __ __ 24 21 __  1  2 __ __
    __ __ __ __ __ __ __ __ __
    END
 
# And
put "\n" xx 60;
 
solveboard q:to/END/;
    0  0  0  0  0  0  0  0  0
    0 11 12 15 18 21 62 61  0
    0  6  0  0  0  0  0 60  0
    0 33  0  0  0  0  0 57  0
    0 32  0  0  0  0  0 56  0
    0 37  0  1  0  0  0 73  0
    0 38  0  0  0  0  0 72  0
    0 43 44 47 48 51 76 77  0
    0  0  0  0  0  0  0  0  0
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
49 50 51 52 53 54 75 76 81
48 47 46 45 44 55 74 77 80
37 38 39 40 43 56 73 78 79
36 35 34 41 42 57 72 71 70
31 32 33 14 13 58 59 68 69
30 17 16 15 12 61 60 67 66
29 18 19 20 11 62 63 64 65
28 25 24 21 10  1  2  3  4
27 26 23 22  9  8  7  6  5
1275 tries


 9 10 13 14 19 20 63 64 65
 8 11 12 15 18 21 62 61 66
 7  6  5 16 17 22 59 60 67
34 33  4  3 24 23 58 57 68
35 32 31  2 25 54 55 56 69
36 37 30  1 26 53 74 73 70
39 38 29 28 27 52 75 72 71
40 43 44 47 48 51 76 77 78
41 42 45 46 49 50 81 80 79
4631 tries
```


Oddly, reversing the tiebreaker rule that makes hidato run twice as fast causes this last example to run four times slower. Go figure...