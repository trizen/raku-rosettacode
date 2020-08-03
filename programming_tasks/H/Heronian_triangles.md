[1]: https://rosettacode.org/wiki/Heronian_triangles

# [Heronian triangles][1]

```perl
sub hero($a, $b, $c) {
    my $s = ($a + $b + $c) / 2;
    my $a2 = $s * ($s - $a) * ($s - $b) * ($s - $c);
    $a2.sqrt;
}
 
sub heronian-area($a, $b, $c) {
    $_ when Int given hero($a, $b, $c).narrow;
} 
 
sub primitive-heronian-area($a, $b, $c) {
    heronian-area $a, $b, $c
        if 1 == [gcd] $a, $b, $c;
}
 
sub show(@measures) {
    say "   Area Perimeter   Sides";
    for @measures -> [$area, $perim, $c, $b, $a] {
	printf "%6d %6d %12s\n", $area, $perim, "$a×$b×$c";
    }
}
 
sub MAIN ($maxside = 200, $first = 10, $witharea = 210) {
    my @h = sort gather
        for 1 .. $maxside -> $c {
            for 1 .. $c -> $b {
                for $c - $b + 1 .. $b -> $a {
                    if primitive-heronian-area($a,$b,$c) -> $area {
                        take [$area, $a+$b+$c, $c, $b, $a];
                    }
                }
            }
        }
 
    say "Primitive Heronian triangles with sides up to $maxside: ", +@h;
 
    say "\nFirst $first:";
    show @h[^$first];
 
    say "\nArea $witharea:";
    show @h.grep: *[0] == $witharea;
}
```

#### Output:
```
Primitive Heronian triangles with sides up to 200: 517

First 10:
   Area Perimeter   Sides
     6     12        3×4×5
    12     16        5×5×6
    12     18        5×5×8
    24     32      4×13×15
    30     30      5×12×13
    36     36      9×10×17
    36     54      3×25×26
    42     42      7×15×20
    60     36     10×13×13
    60     40      8×15×17

Area 210:
   Area Perimeter   Sides
   210     70     17×25×28
   210     70     20×21×29
   210     84     12×35×37
   210     84     17×28×39
   210    140      7×65×68
   210    300    3×148×149
```