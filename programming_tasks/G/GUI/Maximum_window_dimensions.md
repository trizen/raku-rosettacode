[1]: https://rosettacode.org/wiki/GUI/Maximum_window_dimensions

# [GUI/Maximum window dimensions][1]





This is kind-of a silly task. The maximum window size is going to depend on your OS, hardware, display server and graphics toolkit, not your programming language. Taken at face value, using a Linux system running an X11 display server, the maximum displayable window size is the resolution of your monitor. Basically, the size of your desktop viewport. The Raku module X11::libxdo returns the desktop viewport size for get-desktop-dimensions which is the effective maximum window size.

```perl
use X11::libxdo;

my $xdo = Xdo.new;

my ($dw, $dh) = $xdo.get-desktop-dimensions( 0 );

say "Desktop viewport dimensions: (maximum-fullscreen size) $dw x $dh";
```

#### Output:
```
Desktop viewport dimensions: (maximum-fullscreen size) 1920 x 1080
```
