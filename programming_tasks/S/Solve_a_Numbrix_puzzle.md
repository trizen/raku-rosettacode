[1]: http://rosettacode.org/wiki/Solve_a_Numbrix_puzzle

# [Solve a Numbrix puzzle][1]

Using the Warnsdorff solver from [Solve\_a\_Hidato\_puzzle](/wiki/Solve\_a\_Hidato\_puzzle" title="Solve a Hidato puzzle):

```perl
my @adjacent =           [-1, 0],
               [ 0, -1],          [ 0, 1],
                         [ 1, 0];
 
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
```


And

```perl
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
```

#### Output:
```
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