[1]: https://rosettacode.org/wiki/Matrix_with_two_diagonals

# [Matrix with two diagonals][1]

```perl
sub dual-diagonal($n) { ([1, |(0 xx $n-1)], *.rotate(-1) … *[*-1]).map: { [$_ Z|| .reverse] } }
 
.say for dual-diagonal(6);
say '';
.say for dual-diagonal(7);
```

#### Output:
```
[1 0 0 0 0 1]
[0 1 0 0 1 0]
[0 0 1 1 0 0]
[0 0 1 1 0 0]
[0 1 0 0 1 0]
[1 0 0 0 0 1]

[1 0 0 0 0 0 1]
[0 1 0 0 0 1 0]
[0 0 1 0 1 0 0]
[0 0 0 1 0 0 0]
[0 0 1 0 1 0 0]
[0 1 0 0 0 1 0]
[1 0 0 0 0 0 1]
```
