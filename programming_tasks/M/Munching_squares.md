[1]: https://rosettacode.org/wiki/Munching_squares

# [Munching squares][1]





Here's one simple way:

```perl
my $ppm = open("munching0.ppm", :w) orelse .die;

$ppm.print(q :to 'EOT');
P3
256 256
255
EOT

for 0 .. 255 -> $row {
    for 0 .. 255 -> $col {
        my $color = $row +^ $col;
        $ppm.print("0 $color 0 ");
    }
    $ppm.say();
}

$ppm.close();
```


Another way:

```perl
my @colors = map -> $r, $g, $b { Buf.new: $r, $g, $b },
        map -> $x { floor ($x/256) ** 3 * 256 },
            (flat (0...255) Z
             (255...0) Z
             flat (0,2...254),(254,252...0));


my $PPM = open "munching1.ppm", :w orelse .die;

$PPM.print: qq:to/EOH/;
    P6
    # munching.pgm
    256 256
    255
    EOH

$PPM.write: @colors[$_] for ^256 X+^ ^256;

$PPM.close;
```


<span typeof="mw:File">[<img alt="Raku output" src="https://rosettacode.org/w/thumb.php?f=Perl_6_xor_pattern.png&amp;width=200" decoding="async" loading="lazy" width="200" height="200" class="mw-file-element" srcset="https://static.wikitide.net/rosettacodewiki/7/7c/Perl_6_xor_pattern.png 1.5x" />](https://rosettacode.org/wiki/File:Perl_6_xor_pattern.png)</span>
