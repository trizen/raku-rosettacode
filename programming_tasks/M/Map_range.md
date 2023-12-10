[1]: https://rosettacode.org/wiki/Map_range

# [Map range][1]


Return a closure that does the mapping without have to supply the ranges every time.

```perl
sub getmapper(Range $a, Range  $b) {
  my ($a1, $a2) = $a.bounds;
  my ($b1, $b2) = $b.bounds;
  return -> $s { $b1 + (($s-$a1) * ($b2-$b1) / ($a2-$a1)) }
}

my &mapper = getmapper(0 .. 10, -1 .. 0);
for ^11 -> $x {say "$x maps to &mapper($x)"}
```

#### Output:
```
0 maps to -1
1 maps to -0.9
2 maps to -0.8
3 maps to -0.7
4 maps to -0.6
5 maps to -0.5
6 maps to -0.4
7 maps to -0.3
8 maps to -0.2
9 maps to -0.1
10 maps to 0
```
