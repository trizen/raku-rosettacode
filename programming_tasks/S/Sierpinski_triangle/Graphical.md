[1]: https://rosettacode.org/wiki/Sierpinski_triangle/Graphical

# [Sierpinski triangle/Graphical][1]





This is a recursive solution. It is not really practical for more than 8 levels of recursion, but anything more than 7 is barely visible anyway.

```perl
my $levels = 8;
my $side   = 512;
my $height = get_height($side);

sub get_height ($side) { $side * 3.sqrt / 2 }

sub triangle ( $x1, $y1, $x2, $y2, $x3, $y3, $fill?, $animate? ) {
    my $svg;
    $svg ~= qq{<polygon points="$x1,$y1 $x2,$y2 $x3,$y3"};
    $svg ~= qq{ style="fill: $fill; stroke-width: 0;"} if $fill;
    $svg ~= $animate
        ?? qq{>\n  <animate attributeType="CSS" attributeName="opacity"\n  values="1;0;1" keyTimes="0;.5;1" dur="20s" repeatCount="indefinite" />\n</polygon>}
        !! ' />';
    return $svg;
}

sub fractal ( $x1, $y1, $x2, $y2, $x3, $y3, $r is copy ) {
    my $svg;
    $svg ~= triangle( $x1, $y1, $x2, $y2, $x3, $y3 );
    return $svg unless --$r;
    my $side = abs($x3 - $x2) / 2;
    my $height = get_height($side);
    $svg ~= fractal( $x1, $y1-$height*2, $x1-$side/2, $y1-3*$height, $x1+$side/2, $y1-3*$height, $r);
    $svg ~= fractal( $x2, $y1, $x2-$side/2, $y1-$height, $x2+$side/2, $y1-$height, $r);
    $svg ~= fractal( $x3, $y1, $x3-$side/2, $y1-$height, $x3+$side/2, $y1-$height, $r);
}

my $fh = open('sierpinski_triangle.svg', :w) orelse .die;
$fh.print: qq:to/EOD/,
<?xml version="1.0" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "https://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg width="100%" height="100%" version="1.1" xmlns="https://www.w3.org/2000/svg">
<defs>
  <radialGradient id="basegradient" cx="50%" cy="65%" r="50%" fx="50%" fy="65%">
    <stop offset="10%" stop-color="#ff0" />
    <stop offset="60%" stop-color="#f00" />
    <stop offset="99%" stop-color="#00f" />
  </radialGradient>
</defs>
EOD

triangle( $side/2, 0, 0, $height, $side, $height, 'url(#basegradient)' ),
triangle( $side/2, 0, 0, $height, $side, $height, '#000', 'animate' ),
'<g style="fill: #fff; stroke-width: 0;">',
fractal( $side/2, $height, $side*3/4, $height/2, $side/4, $height/2, $levels ),
'</g></svg>';
```
