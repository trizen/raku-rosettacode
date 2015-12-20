[1]: http://rosettacode.org/wiki/Solve_a_Hopido_puzzle

# [Solve a Hopido puzzle][1]

Using the solver from [Solve\_a\_Hidato\_puzzle](/wiki/Solve\_a\_Hidato\_puzzle" title="Solve a Hidato puzzle).

```perl6
my @adjacent = [3, 0],
      [2, -2],         [2, 2],
   [0, -3],                [0, 3],
      [-2, -2],        [-2, 2],
               [-3, 0];
 
solveboard q:to/END/;
    . 0 0 . 0 0 .
    0 0 0 0 0 0 0
    0 0 0 0 0 0 0
    . 0 0 0 0 0 .
    . . 0 0 0 . .
    . . . 1 . . .
    END
```

#### Output:
```
   21  4    20  3   
26 12 15 25 11 14 24
17  6  9 18  5  8 19
   22 27 13 23  2   
      16  7 10      
          1         
59 tries
```