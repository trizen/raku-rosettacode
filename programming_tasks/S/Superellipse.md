[1]: https://rosettacode.org/wiki/Superellipse

# [Superellipse][1]

Generate an svg image to STDOUT. Redirect into a file to capture it.

```perl
constant a = 200;
constant b = 200;
constant n = 2.5;
 
# y in terms of x
sub y ($x) { sprintf "%d", b * (1 - ($x / a).abs ** n ) ** (1/n) }
 
# find point pairs for one quadrant
my @q = flat map -> \x { x, y(x) }, (0, 2 ... 200);
 
# Generate an SVG image
INIT say qq:to/STOP/;
    <?xml version="1.0" standalone="no"?>
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
    <svg height="{b*2}" width="{a*2}" version="1.1" xmlns="http://www.w3.org/2000/svg">
    STOP
END say '</svg>';
 
.put for
pline( @q ),
pline( @q «*» ( 1,-1) ), # flip and mirror
pline( @q «*» (-1,-1) ), # for the other
pline( @q «*» (-1, 1) ); # three quadrants
 
sub pline (@q) {
    qq:to/END/;
    <polyline points="{@q}"
    style="fill:none; stroke:black; stroke-width:3" transform="translate({a}, {b})" />
    END
}
```


See [Superellipse image](https://gist.github.com/thundergnat/cc41a5fae7021803496c#file-superellipse-svg)