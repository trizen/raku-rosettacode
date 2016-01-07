[1]: http://rosettacode.org/wiki/Bitmap/Write_a_PPM_file

# [Bitmap/Write a PPM file][1]

We'll assume the Bitmap and Pixel classes have been imported already.

```perl
role PPM {
    method P6 returns Blob {
	"P6\n{self.width} {self.height}\n255\n".encode('ascii')
	~ Blob.new: flat map { .R, .G, .B }, self.data
    }
}
my Bitmap $b = Bitmap.new( width => 125, height => 125) but PPM;
for ^$b.height X ^$b.width -> $i, $j {
    $b.pixel($i, $j) = Pixel.new: :R($i*2), :G($j*2), :B(255-$i*2);
}
 
$*OUT.write: $b.P6;
```


Converted to a png. (ppm files not locally supported)



[<img alt="Ppm-perl6.png" src="http://rosettacode.org/mw/images/2/27/Ppm-perl6.png" width="125" height="125"/>](http://rosettacode.org/wiki/File:Ppm-perl6.png)