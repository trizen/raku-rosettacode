[1]: https://rosettacode.org/wiki/Pseudorandom_Number_Generator_Image

# [Pseudorandom Number Generator Image][1]

MoarVM [uses Mersenne Twister](https://github.com/MoarVM/MoarVM/blob/master/3rdparty/tinymt/tinymt64.c) as its PRNG but a prime seeder is not mandatory.

```perl
# 20200818 Raku programming solution
 
use Image::PNG::Portable;
 
srand 2⁶³ - 25; # greatest prime smaller than 2⁶³ and the max my system can take
 
my @data = < 250 500 1000 1500 >;
 
@data.map: {
   my $o = Image::PNG::Portable.new: :width($_), :height($_);
   for ^$_ X ^$_ -> @pixel { # about 40% slower if split to ($x,$y) or (\x,\y)
      $o.set: @pixel[0], @pixel[1], 256.rand.Int, 256.rand.Int, 256.rand.Int
   }
   $o.write: "image$_.png" or die;
}
```

#### Output:
```
file image*.png
image1000.png: PNG image data, 1000 x 1000, 8-bit/color RGBA, non-interlaced
image1500.png: PNG image data, 1500 x 1500, 8-bit/color RGBA, non-interlaced
image250.png:  PNG image data, 250 x 250, 8-bit/color RGBA, non-interlaced
image500.png:  PNG image data, 500 x 500, 8-bit/color RGBA, non-interlaced
```


[image500.png](https://github.com/SqrtNegInf/Rosettacode-Perl6-Smoke/blob/master/ref/PNG-image500.png) (sample image, offsite)
