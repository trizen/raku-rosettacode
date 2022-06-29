[1]: https://rosettacode.org/wiki/Goldbach%27s_comet

# [Goldbach's comet][1]

For the stretch, actually generates a plot, doesn't just calculate values to be plotted by a third party application. Deviates slightly from the stretch goal, plots the first two thousand defined values rather than the values up to two thousand that happen to be defined. (More closely matches the [wikipedia example image](https://en.wikipedia.org/wiki/Goldbach%27s_comet).)

```perl
sub G (Int $n) { +(2..$n/2).grep: { .is-prime && ($n - $_).is-prime } }
 
# Task
put "The first 100 G values:\n", (^100).map({ G 2 × $_ + 4 }).batch(10)».fmt("%2d").join: "\n";
 
put "\nG 1_000_000 = ", G 1_000_000;
 
# Stretch
use SVG;
use SVG::Plot;
 
my @x = map 2 × * + 4, ^2000;
my @y = @x.map: &G;
 
'Goldbachs-Comet-Raku.svg'.IO.spurt: SVG.serialize: SVG::Plot.new(
    width       => 1000,
    height      => 500,
    background  => 'white',
    title       => "Goldbach's Comet",
    x           => @x,
    values      => [@y,],
).plot: :points;
 
```

#### Output:
```
The first 100 G values:
 1  1  1  2  1  2  2  2  2  3
 3  3  2  3  2  4  4  2  3  4
 3  4  5  4  3  5  3  4  6  3
 5  6  2  5  6  5  5  7  4  5
 8  5  4  9  4  5  7  3  6  8
 5  6  8  6  7 10  6  6 12  4
 5 10  3  7  9  6  5  8  7  8
11  6  5 12  4  8 11  5  8 10
 5  6 13  9  6 11  7  7 14  6
 8 13  5  8 11  7  9 13  8  9

G 1_000_000 = 5402
```


**Stretch goal:** (offsite SVG image) [Goldbachs-Comet-Raku.svg](https://raw.githubusercontent.com/thundergnat/rc/master/img/Goldbachs-Comet-Raku.svg)
