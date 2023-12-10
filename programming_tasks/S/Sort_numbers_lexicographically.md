[1]: https://rosettacode.org/wiki/Sort_numbers_lexicographically

# [Sort numbers lexicographically][1]





It is somewhat odd that the task name is sort **numbers** lexicographically but immediately backtracks in the task header to sorting **integers** lexicographically.  Why only integers? This will sort ANY real numbers lexicographically. For a non-integer, assumes that the given number is a hard boundary and 1 is a "soft" boundary. E.G. The given number is definitely included; 1 is only a threshold, it is included if it matches exactly. (Could be the other way around, this it the way I choose.)

```perl
sub lex (Real $n, $step = 1) {
    ($n < 1 ?? ($n, * + $step …^ * > 1)
            !! ($n, * - $step …^ * < 1)).sort: ~*
}

# TESTING
for 13, 21, -22, (6, .333), (-4, .25), (-5*π, e) {
    my ($bound, $step) = |$_, 1;
    say "Boundary:$bound, Step:$step >> ", lex($bound, $step).join: ', ';
}
```

#### Output:
```
Boundary:13, Step:1 >> 1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9
Boundary:21, Step:1 >> 1, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 2, 20, 21, 3, 4, 5, 6, 7, 8, 9
Boundary:-22, Step:1 >> -1, -10, -11, -12, -13, -14, -15, -16, -17, -18, -19, -2, -20, -21, -22, -3, -4, -5, -6, -7, -8, -9, 0, 1
Boundary:6, Step:0.333 >> 1.005, 1.338, 1.671, 2.004, 2.337, 2.67, 3.003, 3.336, 3.669, 4.002, 4.335, 4.668, 5.001, 5.334, 5.667, 6
Boundary:-4, Step:0.25 >> -0.25, -0.5, -0.75, -1, -1.25, -1.5, -1.75, -2, -2.25, -2.5, -2.75, -3, -3.25, -3.5, -3.75, -4, 0, 0.25, 0.5, 0.75, 1
Boundary:-15.707963267948966, Step:2.718281828459045 >> -10.271399611030876, -12.989681439489921, -15.707963267948966, -2.116554125653742, -4.834835954112787, -7.553117782571832, 0.6017277028053032
```
