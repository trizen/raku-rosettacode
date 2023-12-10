[1]: https://rosettacode.org/wiki/Longest_common_subsequence

# [Longest common subsequence][1]





### Recursion



This solution is similar to the Haskell one. It is slow.

```perl
say lcs("thisisatest", "testing123testing");sub lcs(Str $xstr, Str $ystr) {
    return "" unless $xstr && $ystr;

    my ($x, $xs, $y, $ys) = $xstr.substr(0, 1), $xstr.substr(1), $ystr.substr(0, 1), $ystr.substr(1);
    return $x eq $y
        ?? $x ~ lcs($xs, $ys)
        !! max(:by{ $^a.chars }, lcs($xstr, $ys), lcs($xs, $ystr) );
}

say lcs("thisisatest", "testing123testing");
```


### Dynamic Programming

```perl
sub lcs(Str $xstr, Str $ystr) {
    my ($xlen, $ylen) = ($xstr, $ystr)>>.chars;
    my @lengths = map {[(0) xx ($ylen+1)]}, 0..$xlen;

    for $xstr.comb.kv -> $i, $x {
        for $ystr.comb.kv -> $j, $y {
            @lengths[$i+1][$j+1] = $x eq $y ?? @lengths[$i][$j]+1 !! (@lengths[$i+1][$j], @lengths[$i][$j+1]).max;
        }
    }

    my @x = $xstr.comb;
    my ($x, $y) = ($xlen, $ylen);
    my $result = "";
    while $x != 0 && $y != 0 {
        if @lengths[$x][$y] == @lengths[$x-1][$y] {
            $x--;
        }
        elsif @lengths[$x][$y] == @lengths[$x][$y-1] {
            $y--;
        }
        else {
            $result = @x[$x-1] ~ $result;
            $x--;
            $y--;
        }
    }

    return $result;
}

say lcs("thisisatest", "testing123testing");
```


### Bit Vector



Bit parallel dynamic programming with nearly linear complexity O(n). It is fast.

```perl
sub lcs(Str $xstr, Str $ystr) {
    my (@a, @b) := ($xstr, $ystr)».comb;
    my (%positions, @Vs, $lcs);

    for @a.kv -> $i, $x { %positions{$x} +|= 1 +< ($i % @a) }

    my $S = +^ 0;
    for (0 ..^ @b) -> $j {
        my $u = $S +& (%positions{@b[$j]} // 0);
        @Vs[$j] = $S = ($S + $u) +| ($S - $u)
    }

    my ($i, $j) = @a-1, @b-1;
    while ($i ≥ 0 and $j ≥ 0) {
        unless (@Vs[$j] +& (1 +< $i)) {
            $lcs [R~]= @a[$i] unless $j and ^@Vs[$j-1] +& (1 +< $i);
            $j--
        }
        $i--
    }
    $lcs
}

say lcs("thisisatest", "testing123testing");
```
