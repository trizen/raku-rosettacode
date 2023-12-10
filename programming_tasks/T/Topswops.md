[1]: https://rosettacode.org/wiki/Topswops

# [Topswops][1]



```perl
sub swops(@a is copy) {
    my int $count = 0;
    until @a[0] == 1 {
        @a[ ^@a[0] ] .= reverse;
        ++$count;
    }
    $count
}

sub topswops($n) { max (1..$n).permutations.race.map: &swops }

say "$_ {topswops $_}" for 1 .. 10;
```

#### Output:
```
1 0
2 1
3 2
4 4
5 7
6 10
7 16
8 22
9 30
10 38
```


Alternately, using string manipulation instead. Much faster, though honestly, still not very fast.

```perl
sub swops($a is copy) {
    my int $count = 0;
    while (my \l = $a.ord) > 1 {
        $a = $a.substr(0, l).flip ~ $a.substr(l);
        ++$count;
    }
    $count
}

sub topswops($n) { max (1..$n).permutations.map: { .chrs.join.&swops } }

say "$_ {topswops $_}" for 1 .. 10;
```


Same output
