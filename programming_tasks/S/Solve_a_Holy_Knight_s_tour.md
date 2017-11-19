[1]: http://rosettacode.org/wiki/Solve_a_Holy_Knight's_tour

# [Solve a Holy Knight's tour][1]

Using the Warnsdorff algorithm from [Solve_a_Hidato_puzzle](http://rosettacode.org/wiki/Solve_a_Hidato_puzzle).

```perl
my @adjacent =
               [ -2, -1],  [ -2, 1],
      [-1,-2],                       [-1,+2],
      [+1,-2],                       [+1,+2],
               [ +2, -1],  [ +2, 1];
 
solveboard q:to/END/;
    . 0 0 0
    . 0 . 0 0
    . 0 0 0 0 0 0 0
    0 0 0 . . 0 . 0
    0 . 0 . . 0 0 0
    1 0 0 0 0 0 0
    . . 0 0 . 0
    . . . 0 0 0
    END
```

#### Output:
```
   25 14 27
   36    24 15
   31 26 13 28 23  6 17
35 12 29       16    22
30    32        7 18  5
 1 34 11  8 19  4 21
       2 33     9
         10  3 20
84 tries
```