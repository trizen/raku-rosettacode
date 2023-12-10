[1]: https://rosettacode.org/wiki/Grayscale_image

# [Grayscale image][1]





This script expects to be fed a P6 .ppm file name at the command line. It will convert it to grey scale and save it as a binary portable grey map (P5 .pgm) file.

```perl
sub MAIN ($filename = 'default.ppm') {

    my $in = open($filename, :r, :enc<iso-8859-1>) or die $in;

    my ($type, $dim, $depth) = $in.lines[^3];

    my $outfile = $filename.subst('.ppm', '.pgm');
    my $out = open($outfile, :w, :enc<iso-8859-1>) or die $out;

    $out.say("P5\n$dim\n$depth");

    for $in.lines.ords -> $r, $g, $b {
        my $gs = $r * 0.2126 + $g * 0.7152 + $b * 0.0722;
        $out.print: chr($gs.floor min 255);
    }

    $in.close;
    $out.close;
}
```


Using the .ppm file from the [Write a PPM file](https://rosettacode.org/wiki/Bitmap/Write_a_PPM_file#Raku) task:



Original: <span class="mw-default-size" typeof="mw:File">[<img src="https://static.wikitide.net/rosettacodewiki/2/27/Ppm-perl6.png" decoding="async" loading="lazy" width="125" height="125" class="mw-file-element" />](https://rosettacode.org/wiki/File:Ppm-perl6.png)</span>   Grey Scale: <span class="mw-default-size" typeof="mw:File">[<img src="https://static.wikitide.net/rosettacodewiki/f/fe/Pgm-g2-perl6.png" decoding="async" loading="lazy" width="125" height="125" class="mw-file-element" />](https://rosettacode.org/wiki/File:Pgm-g2-perl6.png)</span>
