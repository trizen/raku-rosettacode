[1]: http://rosettacode.org/wiki/Julia_set

# [Julia set][1]

```perl
use Image::PNG::Portable;

my ($w, $h) = 800, 600;
my $out = Image::PNG::Portable.new: :width($w), :height($h);

my $maxIter = 255;
my $c = -0.7 + 0.27015i;

julia($out);
$out.write: 'Julia-set-perl6.png';

sub julia ( $png ) {
    for ^$w -> $x {
        for ^$h -> $y {
            my $z = Complex.new(($x - $w / 2) / $w * 3, ($y - $h / 2) / $h * 2);
            my $i = $maxIter;
            while (abs($z) < 2 and --$i) {
                $z = $z*$z + $c;
            }
            $png.set: $x, $y, |hsv2rgb($i / $maxIter * 360, 1, ?$i).reverse;
        }
    }
}

sub hsv2rgb ( $h, $s, $v ){
    my $c = $v * $s;
    my $x = $c * (1 - abs( (($h/60) % 2) - 1 ) );
    my $m = $v - $c;
    my ($r, $g, $b) = do given $h {
        when   0..^60  { $c, $x, 0 }
        when  60..^120 { $x, $c, 0 }
        when 120..^180 { 0, $c, $x }
        when 180..^240 { 0, $x, $c }
        when 240..^300 { $x, 0, $c }
        when 300..^360 { $c, 0, $x }
    }
    ( $r, $g, $b ) = map { (($_+$m) * 255).Int }, $r, $g, $b;
}
```
