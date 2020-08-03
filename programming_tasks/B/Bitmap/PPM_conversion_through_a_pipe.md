[1]: https://rosettacode.org/wiki/Bitmap/PPM_conversion_through_a_pipe

# [Bitmap/PPM conversion through a pipe][1]

```raku
#!/usr/bin/env perl6
 
# Reference:
# https://rosettacode.org/wiki/Bitmap/Write_a_PPM_file#Raku
 
use v6;
 
class Pixel { has uint8 ($.R, $.G, $.B) }
class Bitmap {
    has UInt ($.width, $.height);
    has Pixel @!data;
 
    method fill(Pixel $p) {
        @!data = $p.clone xx ($!width*$!height)
    }
    method pixel(
          $i where ^$!width,
          $j where ^$!height
          --> Pixel
      ) is rw { @!data[$i*$!height + $j] }
 
    method data { @!data }
}
 
role PPM {
    method P6 returns Blob {
        "P6\n{self.width} {self.height}\n255\n".encode('ascii')
        ~ Blob.new: flat map { .R, .G, .B }, self.data
    }
}
 
my Bitmap $b = Bitmap.new(width => 125, height => 125) but PPM;
for flat ^$b.height X ^$b.width -> $i, $j {
    $b.pixel($i, $j) = Pixel.new: :R($i*2), :G($j*2), :B(255-$i*2);
}
 
my $proc = run '/usr/bin/convert','-','output_piped.jpg', :in;
$proc.in.write: $b.P6;
$proc.in.close;
```

#### Output:
```
file output_piped.jpg
output_piped.jpg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, baseline, precision 8, 125x125, frames 3
```