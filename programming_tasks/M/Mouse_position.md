[1]: https://rosettacode.org/wiki/Mouse_position

# [Mouse position][1]



```perl
use java::awt::MouseInfo:from<java>;

given MouseInfo.getPointerInfo.getLocation {
    say .getX, 'x', .getY;
}
```


An implementation that will work on any X11 windowing system. Reports mouse position, window ID# and window name for whichever window the mouse pointer is over. Automatically moves mouse for hands-off testing purposes.

```perl
use X11::libxdo;
my $xdo = Xdo.new;

my ($dw, $dh) = $xdo.get-desktop-dimensions( 0 );

sleep .25;

for ^$dw -> $mx {
    my $my = (($mx / $dh * τ).sin * 500).abs.Int + 200;
    $xdo.move-mouse( $mx, $my, 0 );
    my ($x, $y, $window-id, $screen) = $xdo.get-mouse-info;
    my $name = (try $xdo.get-window-name($window-id) if $window-id)
       // 'No name set';

    my $line = "Mouse location: x=$x : y=$y\nWindow under mouse:\nWindow ID: " ~
       $window-id ~ "\nWindow name: " ~ $name ~ "\nScreen #: $screen";

    print "\e[H\e[J", $line;
    sleep .001;
}

say '';
```
