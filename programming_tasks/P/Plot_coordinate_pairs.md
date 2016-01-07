[1]: http://rosettacode.org/wiki/Plot_coordinate_pairs

# [Plot coordinate pairs][1]

Generate an SVG image file.

```perl
use SVG;
use SVG::Plot;
 
my @x = (0, 1, 2, 3, 4, 5, 6, 7, 8, 9);
my @y = (2.7, 2.8, 31.4, 38.1, 58.0, 76.2, 100.5, 130.0, 149.3, 180.0);
 
my $svg = SVG::Plot.new(
    width       => 512,
    height      => 512,
    x           => @x,
    x-tick-step => { 1 },
    values      => [@y],
    title  => 'Coordinate Pairs',
).plot(:xy-lines);
 
say SVG.serialize($svg);
```


[<img alt="Coordinate-pairs-perl6.svg" src="http://rosettacode.org/mw/images/thumb/b/b2/Coordinate-pairs-perl6.svg/512px-Coordinate-pairs-perl6.svg.png" width="512" height="512" />](http://rosettacode.org/wiki/File:Coordinate-pairs-perl6.svg)