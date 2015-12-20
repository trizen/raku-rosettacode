[1]: http://rosettacode.org/wiki/Bitmap

# [Bitmap][1]

```perl
class Pixel { has UInt ($.R, $.G, $.B) }
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
 
    method set-pixel ($i, $j, Pixel $p) {
	self.pixel($i, $j) = $p.clone;
    }
    method get-pixel ($i, $j) returns Pixel {
	self.pixel($i, $j);
    }
}
 
my Bitmap $b = Bitmap.new( width => 10, height => 10);
 
$b.fill( Pixel.new( R => 0, G => 0, B => 200) );
 
$b.set-pixel( 7, 5, Pixel.new( R => 100, G => 200, B => 0) );
 
say $b.perl;
```


Thanks to the `rw` trait on the `pixel` method, we don't actually need to define two separate methods, `set-pixel` and `get-pixel`, but that is an explicit requirement of the task.  (Beware your presuppositions!  In Perl 6, accessors only determine identity, not use.  In particular, identity is considered orthogonal to lvalue/rvalue context.)