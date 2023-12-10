[1]: https://rosettacode.org/wiki/Draw_pixel_2

# [Draw pixel 2][1]





Coordinates of random pixel displayed in window title. To make the single pixel show up better I filled in the drawing area background with black to get better contrast.

```perl
use GTK::Simple;
use GTK::Simple::DrawingArea;
use Cairo;
my ($w, $h) = 640, 480;
my ($x, $y) = (^$w).pick, (^$h).pick;

my $app = GTK::Simple::App.new(:title("Draw Pixel 2 @ $x,$y"));
my $da  = GTK::Simple::DrawingArea.new;
gtk_simple_use_cairo;

$app.set-content( $da );
$app.border-width = 5;
$da.size-request($w, $h);

sub rect-do( $d, $ctx ) {
    given $ctx {
        .rgb(0, 0, 0);
        .rectangle(0, 0, $w, $h);
        .fill;
        .rgb(1, 1, 0);
        .rectangle($x, $y, 1, 1);
        .fill;
    }
}

my $ctx = $da.add-draw-handler( &rect-do );
$app.run;
```
