[1]: https://rosettacode.org/wiki/Sierpinski_pentagon

# [Sierpinski pentagon][1]

Generate an SVG file to STDOUT. Redirect to a file to capture and display it.

```perl
constant order  = 5;
constant $dim   = 250;
constant $sides = 5;
constant scaling-factor = ( 3 - 5**.5 ) / 2;
my @orders = ((1 - scaling-factor) * $dim) «*» scaling-factor «**» (^order);
 
INIT say qq:to/STOP/;
    <?xml version="1.0" standalone="no"?>
    <!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
    <svg height="{$dim*2}" width="{$dim*2}" style="fill:blue" transform="translate($dim,$dim) rotate(-18)"
      version="1.1" xmlns="http://www.w3.org/2000/svg">
    STOP
END say '</svg>';
 
my @vertices = map { cis( $_ * τ / $sides ) }, ^$sides;
 
for 0 ..^ $sides ** order -> $i {
   my $vector = [+] @vertices[$i.base($sides).fmt("%{order}d").comb] «*» @orders;
   say pgon ((@orders[*-1] * (1 - scaling-factor)) «*» @vertices «+» $vector)».reals».fmt("%0.3f");
};
 
sub pgon (@q) { qq|<polygon points="{@q}"/>| }
 
```


See [5th order pentaflake](https://rosettacode.org/mw/images/5/57/Perl6_pentaflake.svg)