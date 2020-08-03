[1]: https://rosettacode.org/wiki/Color_wheel

# [Color wheel][1]

```raku
use Image::PNG::Portable;
 
my ($w, $h) = 300, 300;
 
my $out = Image::PNG::Portable.new: :width($w), :height($h);
 
my $center = $w/2 + $h/2*i;
 
color-wheel($out);
 
$out.write: 'Color-wheel-perl6.png';
 
sub color-wheel ( $png ) {
    for ^$w -> $x {
        for ^$h -> $y {
            my $vector    = $center - $x - $y*i;
            my $magnitude = $vector.abs * 2 / $w;
            my $direction = ( π + atan2( |$vector.reals ) ) / τ;
            $png.set: $x, $y, |hsv2rgb( $direction, $magnitude, $magnitude < 1 );
        }
    }
}
 
sub hsv2rgb ( $h, $s, $v ){ # inputs normalized 0-1
    my $c = $v * $s;
    my $x = $c * (1 - abs( (($h*6) % 2) - 1 ) );
    my $m = $v - $c;
    my ($r, $g, $b) = do given $h {
        when   0..^(1/6) { $c, $x, 0 }
        when 1/6..^(1/3) { $x, $c, 0 }
        when 1/3..^(1/2) { 0, $c, $x }
        when 1/2..^(2/3) { 0, $x, $c }
        when 2/3..^(5/6) { $x, 0, $c }
        when 5/6..1      { $c, 0, $x }
    }
    ( $r, $g, $b ) = map { (($_+$m) * 255).Int }, $r, $g, $b;
}
```


Until local image uploading is re-enabled, see [Color-wheel-perl6.png](https://github.com/thundergnat/rc/blob/master/img/Color-wheel-perl6.png)