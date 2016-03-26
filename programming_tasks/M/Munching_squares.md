[1]: http://rosettacode.org/wiki/Munching_squares

# [Munching squares][1]

Here's one simple way:

```perl
my $ppm = open("munching.ppm", :w, :bin) or
  die "Can't create munching.ppm: $!";
 
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
		    ((0...255) Z
		     (255...0) Z
		     (0,2...254),(254,252...0));
 
 
my $PPM = open "munching.ppm", :w, :bin or die "Can't create munching.ppm: $!";
 
$PPM.print: qq:to/EOH/;
    P6
    # munching.pgm
    256 256 
    255
    EOH
 
$PPM.write: @colors[$_] for ^256 X+^ ^256;
 
$PPM.close;
```


[<img alt="Perl 6 output" src="http://rosettacode.org/mw/images/thumb/7/7c/Perl_6_xor_pattern.png/200px-Perl_6_xor_pattern.png" width="200" height="200"/>](http://rosettacode.org/wiki/File:Perl_6_xor_pattern.png)