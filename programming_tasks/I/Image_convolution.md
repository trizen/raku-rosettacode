[1]: https://rosettacode.org/wiki/Image_convolution

# [Image convolution][1]





### Perl 5 PDL library

```perl
use PDL:from<Perl5>;
use PDL::Image2D:from<Perl5>;

my $kernel = pdl [[-2, -1, 0],[-1, 1, 1], [0, 1, 2]]; # emboss

my $image = rpic 'frog.png';
my $smoothed = conv2d $image, $kernel, {Boundary => 'Truncate'};
wpic $smoothed, 'frog_convolution.png';
```


Compare offsite images: [frog.png](https://github.com/SqrtNegInf/Rosettacode-Perl6-Smoke/blob/master/ref/frog.png) vs.
[frog_convolution.png](https://github.com/SqrtNegInf/Rosettacode-Perl6-Smoke/blob/master/ref/frog_convolution.png)



### Imagemagick library

```perl
# Note: must install version from github NOT version from CPAN which needs to be updated. 
# Reference:
# https://github.com/azawawi/perl6-magickwand
# http://www.imagemagick.org/Usage/convolve/
 
use v6;
 
use MagickWand;
 
# A new magic wand
my $original = MagickWand.new;
 
# Read an image
$original.read("./Lenna100.jpg") or die;
 
my $o = $original.clone;
 
# using coefficients from kernel "Sobel"
# http://www.imagemagick.org/Usage/convolve/#sobel
$o.convolve( [ 1, 0, -1,
               2, 0, -2,
               1, 0, -1] );
 
$o.write("Lenna100-convoluted.jpg") or die;
 
# And cleanup on exit
LEAVE {
  $original.cleanup   if $original.defined;
  $o.cleanup if $o.defined;
}
```
