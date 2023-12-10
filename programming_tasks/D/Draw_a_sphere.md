[1]: https://rosettacode.org/wiki/Draw_a_sphere

# [Draw a sphere][1]





### Pure Raku



The C code is modified to output a .pgm file.

```perl
my $width = my $height = 255; # must be odd

my @light = normalize([ 3, 2, -5 ]);

my $depth = 255;

sub MAIN ($outfile = 'sphere-perl6.pgm') {
    spurt $outfile, "P5\n$width $height\n$depth\n"; # .pgm header
    my $out = open( $outfile, :a, :bin ) orelse .die;
    $out.write( Blob.new(draw_sphere( ($width-1)/2, .9, .2) ) );
    $out.close;
}

sub normalize (@vec) { @vec »/» ([+] @vec »*« @vec).sqrt }

sub dot (@x, @y) { -([+] @x »*« @y) max 0 }

sub draw_sphere ( $rad, $k, $ambient ) {
    my @pixels[$height];
    my $r2 = $rad * $rad;
    my @range = -$rad .. $rad;
    @range.hyper.map: -> $x {
        my @row[$width];
        @range.map: -> $y {
            if (my $x2 = $x * $x) + (my $y2 = $y * $y) < $r2 {
                my @vector = normalize([$x, $y, ($r2 - $x2 - $y2).sqrt]);
                my $intensity = dot(@light, @vector) ** $k + $ambient;
                my $pixel = (0 max ($intensity * $depth).Int) min $depth;
                @row[$y+$rad] = $pixel;
            }
            else {
                @row[$y+$rad] = 0;
            }
        }
        @pixels[$x+$rad] = @row;
    }
    flat |@pixels.map: *.list;
}
```


### Cairo graphics library

```perl
use Cairo;

given Cairo::Image.create(Cairo::FORMAT_ARGB32, 256, 256) {
    given Cairo::Context.new($_) {

        my Cairo::Pattern::Solid $bg .= create(.5,.5,.5);
        .rectangle(0, 0, 256, 256);
        .pattern($bg);
        .fill;
        $bg.destroy;

        my Cairo::Pattern::Gradient::Radial $shadow .=
           create(105.2, 102.4, 15.6, 102.4,  102.4, 128.0);
        $shadow.add_color_stop_rgba(0, .3, .3, .3, .3);
        $shadow.add_color_stop_rgba(1, .1, .1, .1, .02);
        .pattern($shadow);
        .arc(136.0, 134.0, 110, 0, 2 * pi);
        .fill;
        $shadow.destroy;

        my Cairo::Pattern::Gradient::Radial $sphere .=
           create(115.2, 102.4, 25.6, 102.4,  102.4, 128.0);
        $sphere.add_color_stop_rgba(0, 1, 1, .698, 1);
        $sphere.add_color_stop_rgba(1, .923, .669, .144, 1);
        .pattern($sphere);
        .arc(128.0, 128.0, 110, 0, 2 * pi);
        .fill;
        $sphere.destroy;
    };
    .write_png('sphere2-perl6.png');
}
```


See [sphere2-perl6.png](https://github.com/thundergnat/rc/blob/master/img/sphere2-perl6.png) (offsite .png image)
