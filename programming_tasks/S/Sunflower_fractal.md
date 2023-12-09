[1]: https://rosettacode.org/wiki/Sunflower_fractal

# [Sunflower fractal][1]

This is not really a fractal. It is more accurately an example of a Fibonacci spiral or Phi-packing.



Or, to be completely accurate: It is a variation of a generative [Fermat's spiral](https://en.wikipedia.org/wiki/Fermat%27s_spiral) using the Vogel model to implement phi-packing. See: [https://thatsmaths.com/2014/06/05/sunflowers-and-fibonacci-models-of-efficiency](https://thatsmaths.com/2014/06/05/sunflowers-and-fibonacci-models-of-efficiency/)

```perl
use SVG;
 
my $seeds  = 3000;
my @center = 300, 300;
my $scale  = 5;
 
constant \φ = (3 - 5.sqrt) / 2;
 
my @c = map {
    my ($x, $y) = ($scale * .sqrt) «*« |cis($_ * φ * τ).reals »+« @center;
    [ $x.round(.01), $y.round(.01), (.sqrt * $scale / 100).round(.1) ]
}, 1 .. $seeds;
 
say SVG.serialize(
    svg => [
        :600width, :600height, :style<stroke:yellow>,
        :rect[:width<100%>, :height<100%>, :fill<black>],
        |@c.map( { :circle[:cx(.[0]), :cy(.[1]), :r(.[2])] } ),
    ],
);
```


See: [Phi packing](https://github.com/thundergnat/rc/blob/master/img/phi-packing-perl6.svg) (SVG image)