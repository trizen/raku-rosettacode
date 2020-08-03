[1]: https://rosettacode.org/wiki/Bitmap/Read_a_PPM_file

# [Bitmap/Read a PPM file][1]

Uses pieces from [ Bitmap](https://rosettacode.org/wiki/Bitmap#Raku), [ Write a PPM file](https://rosettacode.org/wiki/Bitmap/Write_a_PPM_file#Raku) and [ Grayscale image](https://rosettacode.org/wiki/Grayscale_image#Raku) tasks. Included here to make a complete, runnable program.

```perl
class Pixel { has UInt ($.R, $.G, $.B) }
class Bitmap {
    has UInt ($.width, $.height);
    has Pixel @.data;
}
 
role PGM {
    has @.GS;
    method P5 returns Blob {
	"P5\n{self.width} {self.height}\n255\n".encode('ascii')
	~ Blob.new: self.GS
    }
}
 
sub load-ppm ( $ppm ) {
    my $fh    = $ppm.IO.open( :enc('ISO-8859-1') );
    my $type  = $fh.get;
    my ($width, $height) = $fh.get.split: ' ';
    my $depth = $fh.get;
    Bitmap.new( width => $width.Int, height => $height.Int,
      data => ( $fh.slurp.ords.rotor(3).map:
        { Pixel.new(R => $_[0], G => $_[1], B => $_[2]) } )
    )
}
 
sub grayscale ( Bitmap $bmp ) {
    $bmp.GS = map { (.R*0.2126 + .G*0.7152 + .B*0.0722).round(1) min 255 }, $bmp.data;
}
 
my $filename = './camelia.ppm';
 
my Bitmap $b = load-ppm( $filename ) but PGM;
 
grayscale($b);
 
'./camelia-gs.pgm'.IO.open(:bin, :w).write: $b.P5;
```


See [camelia](https://github.com/thundergnat/rc/blob/master/img/camelia.png), and [camelia-gs](https://github.com/thundergnat/rc/blob/master/img/camelia-gs.png) images. (converted to .png as .ppm format is not widely supported).