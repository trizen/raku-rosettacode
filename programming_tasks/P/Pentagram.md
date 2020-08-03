[1]: https://rosettacode.org/wiki/Pentagram

# [Pentagram][1]

Generate an SVG file to STDOUT. Redirect to a file to capture and display it.

```perl
constant $dim = 200;
constant $sides = 5;
 
INIT say qq:to/STOP/;
    <?xml version="1.0" standalone="no" ?>
     <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.0//EN"
     "http://www.w3.org/TR/2001/PR-SVG-20010719/DTD/svg10.dtd">
    <svg height="{$dim*2}" width="{$dim*2}" style="" xmlns="http://www.w3.org/2000/svg">
    <rect height="100%" width="100%" style="fill:bisque;" />
    STOP
END say '</svg>';
 
my @vertices = map { 0.9 * $dim * cis($_ * τ / $sides) }, ^$sides;
 
say pline map |*.reals, flat @vertices[0, 2 ... *], @vertices[1, 3 ... *];
 
sub pline (@q) {
  qq:to/STOP/;
    <polyline points="{@q[^@q, 0, 1].fmt("%0.3f")}"
    style="fill:seashell; stroke:blue; stroke-width:3;"
    transform="translate($dim, $dim) rotate(-18)" />
    STOP
}
```


See [Pentagram](https://gist.github.com/thundergnat/70108a5160dd17dfe374#file-pentagram-svg)