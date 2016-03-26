[1]: http://rosettacode.org/wiki/Cut_a_rectangle

# [Cut a rectangle][1]

This is a very dumb, straightforward translation of the C code. It is very slow so we'll interrupt the execution and display the partial output.

```perl
subset Byte of Int where ^256;
my @grid of Byte = 0;
 
my Int ($w, $h, $len);
my Int $cnt = 0;
 
my @next;
my @dir = [0, -1], [-1, 0], [0, 1], [1, 0];
sub walk(Int $y, Int $x) {
    my ($i, $t);
    if !$y || $y == $h || !$x || $x == $w {
        $cnt += 2;
        return;
    }
    $t = $y * ($w + 1) + $x;
    @grid[$t]++, @grid[$len - $t]++;
 
    loop ($i = 0; $i < 4; $i++) {
        if !@grid[$t + @next[$i]] {
            walk($y + @dir[$i][0], $x + @dir[$i][1]);
        }
    }
 
    @grid[$t]--, @grid[$len - $t]--;
}
 
sub solve(Int $hh, Int $ww, Int $recur) returns Int {
    my ($t, $cx, $cy, $x);
    $h = $hh, $w = $ww;
 
    if $h +& 1 { $t = $w, $w = $h, $h = $t; }
    if $h +& 1 { return 0; }
    if $w == 1 { return 1; }
    if $w == 2 { return $h; }
    if $h == 2 { return $w; }
 
    $cy = $h div 2, $cx = $w div 2;
 
    $len = ($h + 1) * ($w + 1);
    @grid = ();
    @grid[$len--] = 0;
 
    @next[0] = -1;
    @next[1] = -$w - 1;
    @next[2] = 1;
    @next[3] = $w + 1;
 
    if $recur { $cnt = 0; }
    loop ($x = $cx + 1; $x < $w; $x++) {
        $t = $cy * ($w + 1) + $x;
        @grid[$t] = 1;
        @grid[$len - $t] = 1;
        walk($cy - 1, $x);
    }
    $cnt++;
 
    if $h == $w {
        $cnt *= 2;
    } elsif !($w +& 1) && $recur {
        solve($w, $h, 0);
    }
 
    return $cnt;
}
 
my ($y, $x);
loop ($y = 1; $y <= 10; $y++) {
    loop ($x = 1; $x <= $y; $x++) {
        if (!($x +& 1) || !($y +& 1)) {
            printf("%d x %d: %d\n", $y, $x, solve($y, $x, 1));
        }
    }
}
```

#### Output:
```
2 x 1: 1
2 x 2: 2
3 x 2: 3
4 x 1: 1
4 x 2: 4
4 x 3: 9
4 x 4: 22
5 x 2: 5
5 x 4: 39
6 x 1: 1
6 x 2: 6
6 x 3: 23
6 x 4: 90
6 x 5: 263
6 x 6: 1018
7 x 2: 7
7 x 4: 151
7 x 6: 2947
8 x 1: 1
8 x 2: 8
8 x 3: 53
8 x 4: 340
8 x 5: 1675
^C
```