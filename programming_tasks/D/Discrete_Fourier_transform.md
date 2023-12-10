[1]: https://rosettacode.org/wiki/Discrete_Fourier_transform

# [Discrete Fourier transform][1]

```perl
sub ft_inner ( @x, $k, $pos_neg_i where * == i|-i ) {
    my @exp := ( $pos_neg_i * tau / +@x * $k ) «*« @x.keys;
    return sum @x »*« 𝑒 «**« @exp;
}
sub dft   ( @x ) { return @x.keys.map: { ft_inner( @x, $_, -i )       } }
sub idft  ( @x ) { return @x.keys.map: { ft_inner( @x, $_,  i ) / +@x } }
sub clean ( @x ) { @x».round(1e-12)».narrow }

my @tests = ( 1, 2-i, -i, -1+2i     ),
            ( 2,   3,  5,     7, 11 ),
;
for @tests -> @x {
    my @x_dft  =  dft(@x);
    my @x_idft = idft(@x_dft);

    say .key.fmt('%6s:'), .value.&clean.fmt('%5s', ', ') for :@x, :@x_dft, :@x_idft;
    say '';
    warn "Round-trip failed" unless ( clean(@x) Z== clean(@x_idft) ).all;
}
```

#### Output:
```
     x:    1,  2-1i,  0-1i, -1+2i
 x_dft:    2, -2-2i,  0-2i,  4+4i
x_idft:    1,  2-1i,  0-1i, -1+2i

     x:    2,     3,     5,     7,    11
 x_dft:   28, -3.38196601125+8.784022634946i, -5.61803398875+2.800168985749i, -5.61803398875-2.800168985749i, -3.38196601125-8.784022634946i
x_idft:    2,     3,     5,     7,    11
```
