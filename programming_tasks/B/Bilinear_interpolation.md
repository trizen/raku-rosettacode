[1]: https://rosettacode.org/wiki/Bilinear_interpolation

# [Bilinear interpolation][1]

```raku
#!/usr/bin/env perl6
 
use v6;
use GD::Raw;
 
# Reference:
# https://github.com/dagurval/perl6-gd-raw
 
my $fh1 = fopen('./Lenna100.jpg', "rb") or die;
my $img1 = gdImageCreateFromJpeg($fh1);
 
my $fh2 = fopen('./Lenna100-larger.jpg',"wb") or die;
 
my $img1X = gdImageSX($img1);
my $img1Y = gdImageSY($img1);
 
my $NewX = $img1X * 1.6;
my $NewY = $img1Y * 1.6;
 
gdImageSetInterpolationMethod($img1, +GD_BILINEAR_FIXED);
 
my $img2 = gdImageScale($img1, $NewX.ceiling, $NewY.ceiling);
 
gdImageJpeg($img2,$fh2,-1);
 
gdImageDestroy($img1);
gdImageDestroy($img2);
 
fclose($fh1);
fclose($fh2);
 
```

#### Output:
```
file Lenna100*
Lenna100.jpg:        JPEG image data, JFIF standard 1.01, resolution (DPI), density 72x72, segment length 16, baseline, precision 8, 512x512, frames 3
Lenna100-larger.jpg: JPEG image data, JFIF standard 1.01, resolution (DPI), density 96x96, segment length 16, comment: "CREATOR: gd-jpeg v1.0 (using IJG JPEG v80), default quality", baseline, precision 8, 820x820, frames 3
```