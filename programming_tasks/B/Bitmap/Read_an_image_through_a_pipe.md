[1]: https://rosettacode.org/wiki/Bitmap/Read_an_image_through_a_pipe

# [Bitmap/Read an image through a pipe][1]

Uses pieces from [ Bitmap](https://rosettacode.org/wiki/Bitmap#Perl_6) and [ Read a PPM file](https://rosettacode.org/wiki/Bitmap/Read_a_PPM_file#Perl_6) tasks. Included here to make a complete, runnable program.



Uses imagemagick convert to pipe the image in.

```raku
class Pixel { has UInt ($.R, $.G, $.B) }
class Bitmap {
    has UInt ($.width, $.height);
    has Pixel @.data;
}
 
role PPM {
    method P6 returns Blob {
	"P6\n{self.width} {self.height}\n255\n".encode('ascii')
	~ Blob.new: flat map { .R, .G, .B }, self.data
    }
}
 
sub getline ( $proc ) {
    my $line = '#'; # skip comment when reading a .png
    $line = $proc.out.get while $line.substr(0,1) eq '#';
    $line;
}
 
my $filename = './camelia.png';
 
my $proc = run 'convert', $filename, 'ppm:-', :enc('ISO-8859-1'), :out;
 
my $type = getline($proc);
my ($width, $height) = getline($proc).split: ' ';
my $depth = getline($proc);
 
my Bitmap $b = Bitmap.new( width => $width.Int, height => $height.Int) but PPM;
 
$b.data = $proc.out.slurp.ords.rotor(3).map:
  { Pixel.new(R => .[0], G => .[1], B => .[2]) };
 
'./camelia.ppm'.IO.open(:bin, :w).write: $b.P6;
```


See [camelia image here](https://github.com/thundergnat/rc/blob/master/img/camelia.png).