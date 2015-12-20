[1]: http://rosettacode.org/wiki/Grayscale_image

# [Grayscale image][1]

This script expects to be fed a P6 .ppm file name at the command line. It will convert it to grey scale and save it as a binary portable grey map (P5 .pgm) file.

```perl
sub MAIN ($filename = 'default.ppm') {
 
    my $in = open($filename, :r, :enc<iso-8859-1>);
 
    my ($type, $dim, $depth) = $in.lines[^3];
 
    my $outfile = $filename.subst('.ppm', '.pgm');
    my $out = open($outfile, :w, :enc<iso-8859-1>);
 
    $out.say("P5\n$dim\n$depth");
 
    for $in.slurp.ords -> $r, $g, $b {
        my $gs = $r * 0.2126 + $g * 0.7152 + $b * 0.0722;
        $out.print: chr($gs min 255);
    }
 
    $in.close;
    $out.close;
}
```


Using the .ppm file from the [Write a PPM file](/wiki/Bitmap/Write\_a\_PPM\_file#Perl\_6" title="Bitmap/Write a PPM file) task:



Original: [<img alt="Ppm-perl6.png" src="/mw/images/2/27/Ppm-perl6.png" width="125" height="125" />](/wiki/File:Ppm-perl6.png" class="image)   Grey Scale: [<img alt="Pgm-g2-perl6.png" src="/mw/images/f/fe/Pgm-g2-perl6.png" width="125" height="125" />](/wiki/File:Pgm-g2-perl6.png" class="image)