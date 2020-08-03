[1]: https://rosettacode.org/wiki/Perlin_noise

# [Perlin noise][1]

```raku
constant @p = map {:36($_)}, flat <
47 4G 3T 2J 2I F 3N D 5L 2N 2O 1H 5E 6H 7 69 3W 10 2V U 1X 3Y 8 2R 11 6O L A N
5A 6 44 6V 3C 6I 23 0 Q 5H 1Q 2M 70 63 5N 39 Z B W 1L 4X X 2G 6L 45 1K 2F 4U K
3H 3S 4R 4O 1W 4V 22 4L 1Z 3Q 3V 1C R 4M 25 42 4E 6F 2B 33 6D 3E 1O 5V 3P 6E 64
2X 2K 15 1J 1A 6T 14 6S 2U 3Z 1I 1T P 1R 4H 1 60 28 21 5T 24 3O 57 5S 2H I 4P
5K 5G 3R 3M 38 58 4F 2E 4K 2S 31 5I 4T 56 3 1S 1G 61 6A 6Y 3G 3F 5 5M 12 43 3A
3I 73 2A 2D 5W 5R 5Q 1N 6B 1B G 1M H 52 59 S 16 67 53 4Q 5X 3B 6W 48 2 18 4A 4J
1Y 65 49 2T 4B 4N 17 4S 9 3L M 13 71 J 2Q 30 32 27 35 68 6G 4Y 55 34 2W 62 6U
2P 6C 6Z Y 6Q 5D 6M 5U 40 C 5B 4Z 4I 6P 29 1F 41 6J 6X E 6N 2Z 1D 5C 5Y V 51 5J
2Y 4D 54 2C 5O 4W 37 3D 1E 19 3J 4 46 72 3U 6K 5P 2L 66 36 1V T O 20 6R 3X 3K
5F 26 1U 5Z 1P 4C 50
> xx 2;
 
sub fade($_) { $_ * $_ * $_ * ($_ * ($_ * 6 - 15) + 10) }
sub lerp($t, $a, $b) { $a + $t * ($b - $a) }
sub grad($h is copy, $x, $y, $z) {
    $h +&= 15;
    my $u = $h < 8 ?? $x !! $y;
    my $v = $h < 4 ?? $y !! $h == 12|14 ?? $x !! $z;
    ($h +& 1 ?? -$u !! $u) + ($h +& 2 ?? -$v !! $v);
}
 
sub noise($x is copy, $y is copy, $z is copy) is export {
    my ($X, $Y, $Z) = ($x, $y, $z)».floor »+&» 255;
    my ($u, $v, $w) = map &fade, $x -= $X, $y -= $Y, $z -= $Z;
    my ($AA, $AB) = @p[$_] + $Z, @p[$_ + 1] + $Z given @p[$X] + $Y;
    my ($BA, $BB) = @p[$_] + $Z, @p[$_ + 1] + $Z given @p[$X + 1] + $Y;
    lerp($w, lerp($v, lerp($u, grad(@p[$AA    ], $x    , $y    , $z     ),  
                               grad(@p[$BA    ], $x - 1, $y    , $z     )), 
                      lerp($u, grad(@p[$AB    ], $x    , $y - 1, $z     ),  
                               grad(@p[$BB    ], $x - 1, $y - 1, $z     ))),
             lerp($v, lerp($u, grad(@p[$AA + 1], $x    , $y    , $z - 1 ),  
                               grad(@p[$BA + 1], $x - 1, $y    , $z - 1 )), 
                      lerp($u, grad(@p[$AB + 1], $x    , $y - 1, $z - 1 ),
                               grad(@p[$BB + 1], $x - 1, $y - 1, $z - 1 ))));
}
 
say noise 3.14, 42, 7;
```

#### Output:
```
0.13691995878
```