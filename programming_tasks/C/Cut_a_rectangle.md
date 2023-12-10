[1]: https://rosettacode.org/wiki/Cut_a_rectangle

# [Cut a rectangle][1]



```perl
sub solve($hh, $ww, $recurse) {
    my ($h, $w, $t, @grid) = $hh, $ww, 0;
    state $cnt;
    $cnt = 0 if $recurse;

    ($t, $w, $h) = ($w, $h, $w) if $h +& 1;
    return 0  if $h == 1;
    return 1  if $w == 1;
    return $h if $w == 2;
    return $w if $h == 2;

    my ($cy, $cx) = ($h, $w) «div» 2;
    my $len = ($h + 1) × ($w + 1);
    @grid[$len--] = 0;
    my @next = -1, -$w-1, 1, $w+1;

    for $cx+1 ..^ $w -> $x {
        $t = $cy × ($w + 1) + $x;
        @grid[$_] = 1 for $t, $len-$t;
        walk($cy - 1, $x);
    }

    sub walk($y, $x) {
        constant @dir = <0 -1 0 1> Z <-1 0 1 0>;
        $cnt += 2 and return if not $y or $y == $h or not $x or $x == $w;
        my $t = $y × ($w+1) + $x;
        @grid[$_]++ for $t, $len-$t;
        walk($y + @dir[$_;0], $x + @dir[$_;1]) if not @grid[$t + @next[$_]] for 0..3;
        @grid[$_]-- for $t, $len-$t;
    }

    $cnt++;
    if    $h == $w                 { $cnt ×= 2            }
    elsif $recurse and not $w +& 1 { solve($w, $h, False) }
    $cnt
}

((1..9 X 1..9).grep:{ .[0] ≥ .[1] }).flat.map: -> $y, $x {
   say "$y × $x: " ~ solve $y, $x, True unless $x +& 1 and $y +& 1;
}
```

#### Output:
```
2 × 1: 1
2 × 2: 2
3 × 2: 3
4 × 1: 1
4 × 2: 4
4 × 3: 9
4 × 4: 22
5 × 2: 5
5 × 4: 39
6 × 1: 1
6 × 2: 6
6 × 3: 23
6 × 4: 90
6 × 5: 263
6 × 6: 1018
7 × 2: 7
7 × 4: 151
7 × 6: 2947
8 × 1: 1
8 × 2: 8
8 × 3: 53
8 × 4: 340
8 × 5: 1675
8 × 6: 11174
8 × 7: 55939
8 × 8: 369050
9 × 2: 9
9 × 4: 553
9 × 6: 31721
9 × 8: 1812667
```
