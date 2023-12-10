[1]: https://rosettacode.org/wiki/Solve_triangle_solitare_puzzle

# [Solve triangle solitare puzzle][1]



```perl
constant @start =  <
        0
       1 1
      1 1 1
     1 1 1 1
    1 1 1 1 1
>».Int;

constant @moves =
    [ 0, 1, 3],[ 0, 2, 5],[ 1, 3, 6],
    [ 1, 4, 8],[ 2, 4, 7],[ 2, 5, 9],
    [ 3, 4, 5],[ 3, 6,10],[ 3, 7,12],
    [ 4, 7,11],[ 4, 8,13],[ 5, 8,12],
    [ 5, 9,14],[ 6, 7, 8],[ 7, 8, 9],
    [10,11,12],[11,12,13],[12,13,14];

my $format = (1..5).map: {' ' x 5-$_, "%d " x $_, "\n"};

sub solve(@board, @move) {
    return "   Solved" if @board.sum == 1;
    return Nil if @board[@move[1]] == 0;
    my $valid = do given @board[@move[0]] {
        when 0 {
            return Nil if @board[@move[2]] == 0;
            "move {@move[2]} to {@move[0]}\n ";
        }
        default {
            return Nil if @board[@move[2]] == 1;
            "move {@move[0]} to {@move[2]}\n ";
        }
    }

    my @new-layout = @board;
    @new-layout[$_] = 1 - @new-layout[$_] for @move;
    my $result;
    for @moves -> @this-move {
        $result = solve(@new-layout, @this-move);
        last if $result
    }
    $result ?? "$valid\n " ~ sprintf($format, |@new-layout) ~ $result !! $result
}

print "Starting with\n ", sprintf($format, |@start);

my $result;
for @moves -> @this-move {
    $result = solve(@start, @this-move);
    last if $result
};
say $result ?? $result !! "No solution found";
```

#### Output:
```
Starting with
      0  
     1 1  
    1 1 1  
   1 1 1 1  
  1 1 1 1 1  
move 3 to 0
 
      1  
     0 1  
    0 1 1  
   1 1 1 1  
  1 1 1 1 1  
move 8 to 1
 
      1  
     1 1  
    0 0 1  
   1 1 0 1  
  1 1 1 1 1  
move 10 to 3
 
      1  
     1 1  
    1 0 1  
   0 1 0 1  
  0 1 1 1 1  
move 1 to 6
 
      1  
     0 1  
    0 0 1  
   1 1 0 1  
  0 1 1 1 1  
move 11 to 4
 
      1  
     0 1  
    0 1 1  
   1 0 0 1  
  0 0 1 1 1  
move 2 to 7
 
      1  
     0 0  
    0 0 1  
   1 1 0 1  
  0 0 1 1 1  
move 9 to 2
 
      1  
     0 1  
    0 0 0  
   1 1 0 0  
  0 0 1 1 1  
move 0 to 5
 
      0  
     0 0  
    0 0 1  
   1 1 0 0  
  0 0 1 1 1  
move 6 to 8
 
      0  
     0 0  
    0 0 1  
   0 0 1 0  
  0 0 1 1 1  
move 13 to 11
 
      0  
     0 0  
    0 0 1  
   0 0 1 0  
  0 1 0 0 1  
move 5 to 12
 
      0  
     0 0  
    0 0 0  
   0 0 0 0  
  0 1 1 0 1  
move 11 to 13
 
      0  
     0 0  
    0 0 0  
   0 0 0 0  
  0 0 0 1 1  
move 14 to 12
 
      0  
     0 0  
    0 0 0  
   0 0 0 0  
  0 0 1 0 0  
   Solved
```
