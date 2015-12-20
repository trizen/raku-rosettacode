[1]: http://rosettacode.org/wiki/Superellipse

# [Superellipse][1]

Generate an svg image to STDOUT. Redirect into a file to capture it.

```perl
constant a = 200;
constant b = 200;
constant n = 2.5;
 
# y in terms of x
sub y ($x) { sprintf "%d", b * (1 - ($x / a).abs ** n ) ** (1/n) }
 
# find point pairs for one quadrant
my @q = flat map { $_, y $_ }, (0, 5 ... 200);
 
# Generate an SVG image
print qq:to/END/;
    <svg height="{b*2}" width="{a*2}">
    END
pline @q;
pline @q «*» ( 1,-1); # flip and mirror
pline @q «*» (-1,-1); # for the other 
pline @q «*» (-1, 1); # three quadrants
say '</svg>';
 
sub pline (*@q) {
  print qq:to/END/;
    <polyline points="{@q}"
    style="fill:none;stroke:black;stroke-width:3"
    transform="translate({a}, {b})" />
    END
}
```