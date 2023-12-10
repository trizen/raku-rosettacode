[1]: https://rosettacode.org/wiki/Video_display_modes

# [Video display modes][1]


Raku runs on several different operating systems over many different architectures so can not easily assume direct control over hardware. Instead, like most modern programming languages, it relies on the current OS and windowing system to provide APIs.



Here is an example which will work for most Debian based Linuxs (and probably others) running some variant of X11 with a single active monitor.

```perl
my @info = QX('xrandr -q').lines;

@info[0] ~~ /<?after 'current '>(\d+) ' x ' (\d+)/;
my $current = "$0x$1";

my @resolutions;
@resolutions.push: $0 if $_ ~~ /^\s+(\d+'x'\d+)/ for @info;

QX("xrandr -s @resolutions[*-1]");
say "Current resolution {@resolutions[*-1]}.";
for 9 ... 1 {
    print "\rChanging back in $_ seconds...";
    sleep 1;
}
QX("xrandr -s $current");
say "\rResolution returned to {$current}.     ";
```
