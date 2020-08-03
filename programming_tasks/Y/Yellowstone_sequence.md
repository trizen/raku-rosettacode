[1]: https://rosettacode.org/wiki/Yellowstone_sequence

# [Yellowstone sequence][1]


Not really clear whether a line graph or bar graph was desired, so generate both. Also, 100 points don't really give a good feel for the overall shape so do 500.

```perl
my @yellowstone = 1, 2, 3, -> $q, $p {
    state @used = True xx 4;
    state $min  = 3;
    my \index = ($min .. *).first: { not @used[$_] and $_ gcd $q != 1 and $_ gcd $p == 1 };
    @used[index] = True;
    $min = @used.first(!*, :k) // +@used - 1;
    index
} … *;

put "The first 30 terms in the Yellowstone sequence:\n", @yellowstone[^30];

use SVG;
use SVG::Plot;

my @x = ^500;

my $chart = SVG::Plot.new(
    background  => 'white',
    width       => 1000,
    height      => 600,
    plot-width  => 950,
    plot-height => 550,
    x           => @x,
    x-tick-step => { 10 },
    y-tick-step => { 50 },
    min-y-axis  => 0,
    values      => [@yellowstone[@x],],
    title       => "Yellowstone Sequence - First {+@x} values (zero indexed)",
);

my $line = './Yellowstone-sequence-line-perl6.svg'.IO;
my $bars = './Yellowstone-sequence-bars-perl6.svg'.IO;

$line.spurt: SVG.serialize: $chart.plot: :lines;
$bars.spurt: SVG.serialize: $chart.plot: :bars;
```

#### Output:
```
The first 30 terms in the Yellowstone sequence:
1 2 3 4 9 8 15 14 5 6 25 12 35 16 7 10 21 20 27 22 39 11 13 33 26 45 28 51 32 17
```


See (offsite SVG images) [Line graph](https://github.com/thundergnat/rc/blob/master/img/Yellowstone-sequence-line-perl6.svg) or [Bar graph](https://github.com/thundergnat/rc/blob/master/img/Yellowstone-sequence-bars-perl6.svg)
