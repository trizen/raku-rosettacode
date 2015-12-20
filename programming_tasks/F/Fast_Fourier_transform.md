[1]: http://rosettacode.org/wiki/Fast_Fourier_transform

# [Fast Fourier transform][1]

```perl6
sub fft {
    return @_ if @_ == 1;
    my @evn = fft( @_[0, 2 ... *] );
    my @odd = fft( @_[1, 3 ... *] ) Z*
    map &cis, (0, tau / @_ ... *);
    return flat @evn »+« @odd, @evn »-« @odd;
}
 
my @seq    = ^16;
my $cycles = 3;
my @wave   = map { sin( tau * $_ / @seq * $cycles ) }, @seq;
say "wave: ", @wave.fmt("%7.3f");
 
say "fft:  ", fft(@wave)».abs.fmt("%7.3f");
```

#### Output:
```
wave:   0.000   0.924   0.707  -0.383  -1.000  -0.383   0.707   0.924   0.000  -0.924  -0.707   0.383   1.000   0.383  -0.707  -0.924
fft:    0.000   0.000   0.000   8.000   0.000   0.000   0.000   0.000   0.000   0.000   0.000   0.000   0.000   8.000   0.000   0.000
```