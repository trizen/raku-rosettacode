[1]: https://rosettacode.org/wiki/Exactly_three_adjacent_3_in_lists

# [Exactly three adjacent 3 in lists][1]

Generalized

```perl
for 1 .. 4 -> $n {

    say "\nExactly $n {$n}s, and they are consecutive:";

    say .gist, ' ', lc (.Bag{$n} == $n) && ( so .rotor($n=>-($n - 1)).grep: *.all == $n ) for
    [9,3,3,3,2,1,7,8,5],
    [5,2,9,3,3,7,8,4,1],
    [1,4,3,6,7,3,8,3,2],
    [1,2,3,4,5,6,7,8,9],
    [4,6,8,7,2,3,3,3,1],
    [3,3,3,1,2,4,5,1,3],
    [0,3,3,3,3,7,2,2,6],
    [3,3,3,3,3,4,4,4,4]
}
```

#### Output:
```
Exactly 1 1s, and they are consecutive:
[9 3 3 3 2 1 7 8 5] true
[5 2 9 3 3 7 8 4 1] true
[1 4 3 6 7 3 8 3 2] true
[1 2 3 4 5 6 7 8 9] true
[4 6 8 7 2 3 3 3 1] true
[3 3 3 1 2 4 5 1 3] false
[0 3 3 3 3 7 2 2 6] false
[3 3 3 3 3 4 4 4 4] false

Exactly 2 2s, and they are consecutive:
[9 3 3 3 2 1 7 8 5] false
[5 2 9 3 3 7 8 4 1] false
[1 4 3 6 7 3 8 3 2] false
[1 2 3 4 5 6 7 8 9] false
[4 6 8 7 2 3 3 3 1] false
[3 3 3 1 2 4 5 1 3] false
[0 3 3 3 3 7 2 2 6] true
[3 3 3 3 3 4 4 4 4] false

Exactly 3 3s, and they are consecutive:
[9 3 3 3 2 1 7 8 5] true
[5 2 9 3 3 7 8 4 1] false
[1 4 3 6 7 3 8 3 2] false
[1 2 3 4 5 6 7 8 9] false
[4 6 8 7 2 3 3 3 1] true
[3 3 3 1 2 4 5 1 3] false
[0 3 3 3 3 7 2 2 6] false
[3 3 3 3 3 4 4 4 4] false

Exactly 4 4s, and they are consecutive:
[9 3 3 3 2 1 7 8 5] false
[5 2 9 3 3 7 8 4 1] false
[1 4 3 6 7 3 8 3 2] false
[1 2 3 4 5 6 7 8 9] false
[4 6 8 7 2 3 3 3 1] false
[3 3 3 1 2 4 5 1 3] false
[0 3 3 3 3 7 2 2 6] false
[3 3 3 3 3 4 4 4 4] true
```
