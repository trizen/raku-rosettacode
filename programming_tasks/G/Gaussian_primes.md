[1]: https://rosettacode.org/wiki/Gaussian_primes

# [Gaussian primes][1]

Plotting the points up to a radius of 150.

```perl
use List::Divvy;

my @next = { :1x, :1y, :2n },;

sub next-interval (Int $int) {
     @next.append: (^$int).map: { %( :x($int), :y($_), :n($int² + .²) ) };
     @next = |@next.sort: *.<n>;
}

my @gaussian = lazy gather {
    my $interval = 1;
    loop {
        my @this = @next.shift;
        @this.push: @next.shift while @next and @next[0]<n> == @this[0]<n>;
        for @this {
            .take if .<n>.is-prime || (!.<y> && .<x>.is-prime && (.<x> - 3) %% 4);
            next-interval(++$interval) if $interval == .<x>
        }
    }
}

# Primes within a radius of 10 from origin
say "Gaussian primes with a norm less than 100 sorted by norm:";
say @gaussian.&before(*.<n> > 10²).map( {
     my (\i, \j) = .<x y>;
    flat ((i,j),(-i,j),(-i,-j),(i,-j),(j,i),(-j,i),(-j,-i),(j,-i)).map: {
        .[0] ?? .[1] ?? (sprintf "%d%s%di", .[0], (.[1] ≥ 0 ?? '+' !! ''), .[1]) !! .[0] !! "{.[1]}i"
    }} )».subst('1i', 'i', :g)».fmt("%6s")».unique.flat.batch(10).join: "\n" ;


# Plot points within a 150 radius
use SVG;

my @points = unique flat @gaussian.&before(*.<n> > 150²).map: {
    my (\i, \j) = .<x y>;
    do for (i,j),(-i,j),(-i,-j),(i,-j),(j,i),(-j,i),(-j,-i),(j,-i) {
        :use['xlink:href'=>'#point', 'transform'=>"translate({500 + 3 × .[0]},{500 + 3 × .[1]})"]
    }
}

'gaussian-primes-raku.svg'.IO.spurt: SVG.serialize(
    svg => [
        :width<1000>, :height<1000>,
        :rect[:width<100%>, :height<100%>, :style<fill:black;>],
        :defs[:g[:id<point>, :circle[:0cx, :0cy, :2r, :fill('gold')]]],
        |@points
    ],
);
```

#### Output:
```
Gaussian primes with a norm less than 100 sorted by norm:
   1+i   -1+i   -1-i    1-i    2+i   -2+i   -2-i    2-i   1+2i  -1+2i
 -1-2i   1-2i      3     -3     3i    -3i   3+2i  -3+2i  -3-2i   3-2i
  2+3i  -2+3i  -2-3i   2-3i    4+i   -4+i   -4-i    4-i   1+4i  -1+4i
 -1-4i   1-4i   5+2i  -5+2i  -5-2i   5-2i   2+5i  -2+5i  -2-5i   2-5i
   6+i   -6+i   -6-i    6-i   1+6i  -1+6i  -1-6i   1-6i   5+4i  -5+4i
 -5-4i   5-4i   4+5i  -4+5i  -4-5i   4-5i      7     -7     7i    -7i
  7+2i  -7+2i  -7-2i   7-2i   2+7i  -2+7i  -2-7i   2-7i   6+5i  -6+5i
 -6-5i   6-5i   5+6i  -5+6i  -5-6i   5-6i   8+3i  -8+3i  -8-3i   8-3i
  3+8i  -3+8i  -3-8i   3-8i   8+5i  -8+5i  -8-5i   8-5i   5+8i  -5+8i
 -5-8i   5-8i   9+4i  -9+4i  -9-4i   9-4i   4+9i  -4+9i  -4-9i   4-9i
```


Off-site SVG image: [gaussian-primes-raku.svg](https://raw.githubusercontent.com/thundergnat/rc/master/img/gaussian-primes-raku.svg)
