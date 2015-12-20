[1]: http://rosettacode.org/wiki/Pentagram

# [Pentagram][1]

Generate an SVG file to STDOUT. Redirect to a file to capture and display it.

```perl
constant $dim = 200;
constant $sides = 5;
 
INIT say qq:to/STOP/;
    <?xml version="1.0" standalone="no" ?>
     <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.0//EN"
     "http://www.w3.org/TR/2001/PR-SVG-20010719/DTD/svg10.dtd">
    <svg height="{$dim*2}" width="{$dim*2}" xmlns="http://www.w3.org/2000/svg">
    STOP
END say '</svg>';
 
my @vertices = map { 0.9 * $dim * cis($_ * 2 * pi / $sides) }, ^$sides;
@vertices = map |*.reals, flat @vertices[0, 2 ... *], @vertices[1, 3 ... *];
 
say pline @vertices;
for @vertices.rotor(4 => -2, :partial) -> @p {say pline @p};
 
sub pline (*@q) {
  qq:to/STOP/;
    <polyline points="@q[|^@q, 0, 1].fmt("%0.3f")"
    style="fill:seashell;stroke:{<black red orange blue purple green>[$++ % 6]};stroke-width:2"
    transform="translate($dim, $dim) rotate(180)" />
    STOP
}
```