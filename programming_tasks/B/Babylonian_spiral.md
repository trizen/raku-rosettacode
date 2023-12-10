[1]: https://rosettacode.org/wiki/Babylonian_spiral

# [Babylonian spiral][1]

### Translation

```perl
sub babylonianSpiral (\nsteps) {
    my @squareCache = (0..nsteps).hyper.map: *²;
    my @dxys = [0, 0], [0, 1];
    my $dsq  = 1;

    for ^(nsteps-2) {
        my \Θ = atan2 |@dxys[*-1][1,0];
        my @candidates;

        until @candidates.elems {
            $dsq++;
            for @squareCache.kv -> \i, \a {
                last if a > $dsq/2;
                for reverse 0 .. $dsq.sqrt.ceiling -> \j {
                    last if $dsq > (a + my \b = @squareCache[j]);
                    next if $dsq != a + b;
                    @candidates.append: [i, j], [-i, j], [i, -j], [-i, -j],
                                        [j, i], [-j, i], [j, -i], [-j, -i]
                }
            }
        }
        @dxys.push: @candidates.min: { ( Θ - atan2 |.[1,0] ) % τ };
    }

    [\»+«] @dxys
}

# The task
say "The first $_ Babylonian spiral points are:\n",
(babylonianSpiral($_).map: { sprintf '(%3d,%4d)', @$_ }).batch(10).join("\n") given 40;

# Stretch
use SVG;

'babylonean-spiral-raku.svg'.IO.spurt: SVG.serialize(
    svg => [
        :width<100%>, :height<100%>,
        :rect[:width<100%>, :height<100%>, :style<fill:white;>],
        :polyline[ :points(flat babylonianSpiral(10000)),
          :style("stroke:red; stroke-width:6; fill:white;"),
          :transform("scale (.05, -.05) translate (1000,-10000)")
        ],
    ],
);
```

#### Output:
```
The first 40 Babylonian spiral points are:
(  0,   0) (  0,   1) (  1,   2) (  3,   2) (  5,   1) (  7,  -1) (  7,  -4) (  6,  -7) (  4, -10) (  0, -10)
( -4,  -9) ( -7,  -6) ( -9,  -2) ( -9,   3) ( -8,   8) ( -6,  13) ( -2,  17) (  3,  20) (  9,  20) ( 15,  19)
( 21,  17) ( 26,  13) ( 29,   7) ( 29,   0) ( 28,  -7) ( 24, -13) ( 17, -15) ( 10, -12) (  4,  -7) (  4,   1)
(  5,   9) (  7,  17) ( 13,  23) ( 21,  26) ( 28,  21) ( 32,  13) ( 32,   4) ( 31,  -5) ( 29, -14) ( 24, -22)
```


### Independent implementation



Exact same output; about one tenth the execution time.

```perl
my @next = { :x(1), :y(1), :2hyp },;

sub next-interval (Int $int) {
     @next.append: (0..$int).map: { %( :x($int), :y($_), :hyp($int² + .²) ) };
     @next = |@next.sort: *.<hyp>;
}

my @spiral = [\»+«] lazy gather {
    my $interval = 1;
    take [0,0];
    take my @tail = 0,1;
    loop {
        my \Θ = atan2 |@tail[1,0];
        my @this = @next.shift;
        @this.push: @next.shift while @next and @next[0]<hyp> == @this[0]<hyp>;
        my @candidates = @this.map: {
            my (\i, \j) = .<x y>;
            next-interval(++$interval) if $interval == i;
            |((i,j),(-i,j),(i,-j),(-i,-j),(j,i),(-j,i),(j,-i),(-j,-i))
        }
        take @tail = |@candidates.min: { ( Θ - atan2 |.[1,0] ) % τ };
    }
}

# The task
say "The first $_ Babylonian spiral points are:\n",
@spiral[^$_].map({ sprintf '(%3d,%4d)', |$_ }).batch(10).join: "\n" given 40;

# Stretch
use SVG;

'babylonean-spiral-raku.svg'.IO.spurt: SVG.serialize(
    svg => [
        :width<100%>, :height<100%>,
        :rect[:width<100%>, :height<100%>, :style<fill:white;>],
        :polyline[ :points(flat @spiral[^10000]),
          :style("stroke:red; stroke-width:6; fill:white;"),
          :transform("scale (.05, -.05) translate (1000,-10000)")
        ],
    ],
);
```
