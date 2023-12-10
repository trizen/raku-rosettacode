[1]: https://rosettacode.org/wiki/Plot_coordinate_pairs

# [Plot coordinate pairs][1]





Generate an SVG image file.

```perl
use SVG;
use SVG::Plot;

my @x = 0..9;
my @y = (2.7, 2.8, 31.4, 38.1, 58.0, 76.2, 100.5, 130.0, 149.3, 180.0);

say SVG.serialize: SVG::Plot.new(
    width       => 512,
    height      => 512,
    x           => @x,
    x-tick-step => { 1 },
    min-y-axis  => 0,
    values      => [@y,],
    title  => 'Coordinate Pairs',
).plot(:lines);
```


<span class="mw-default-size" typeof="mw:File">[<img src="https://rosettacode.org/w/thumb.php?f=Coordinate-pairs-perl6.svg&amp;width=512" decoding="async" loading="lazy" width="512" height="512" class="mw-file-element" srcset="/w/thumb.php?f=Coordinate-pairs-perl6.svg&amp;width=768 1.5x, /w/thumb.php?f=Coordinate-pairs-perl6.svg&amp;width=1024 2x" />](https://rosettacode.org/wiki/File:Coordinate-pairs-perl6.svg)</span>
