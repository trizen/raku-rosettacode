[1]: https://rosettacode.org/wiki/Chaos_game

# [Chaos game][1]

```perl
use Image::PNG::Portable;
 
my ($w, $h) = (640, 640);
 
my $png = Image::PNG::Portable.new: :width($w), :height($h);
 
my @vertex = [0, 0], [$w, 0], [$w/2, $h];
 
my ($x, $y) = (0, 0);
 
for ^1e5 {
    ($x, $y) = do given @vertex.pick -> @v { ((($x, $y) »+« @v) »/» 2)».Int };
    $png.set: $x, $y, 0, 255, 0;
}
 
$png.write: 'Chaos-game-perl6.png';
```