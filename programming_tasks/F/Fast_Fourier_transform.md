[1]: https://rosettacode.org/wiki/Fast_Fourier_transform

# [Fast Fourier transform][1]

```raku
sub fft {
    return @_ if @_ == 1;
    my @evn = fft( @_[0, 2 ... *] );
    my @odd = fft( @_[1, 3 ... *] ) Z*
    map &cis, (0, -tau / @_ ... *);
    return flat @evn »+« @odd, @evn »-« @odd;
}
 
.say for fft <1 1 1 1 0 0 0 0>;
```

#### Output:
```
4+0i
1-2.41421356237309i
0+0i
1-0.414213562373095i
0+0i
1+0.414213562373095i
0+0i
1+2.41421356237309i
```


For the fun of it, here is a purely functional version:

```raku
sub fft {
    @_ == 1 ?? @_ !!
    fft(@_[0,2...*]) «+«
    fft(@_[1,3...*]) «*« map &cis, (0,-τ/@_...^-τ)
}
```


This particular version is numerically inaccurate though, because of the pi approximation. It is possible to fix it with the 'cisPi' function available in the TrigPi module:

```raku
sub fft {
    use TrigPi;
    @_ == 1 ?? @_ !!
    fft(@_[0,2...*]) «+«
    fft(@_[1,3...*]) «*« map &cisPi, (0,-2/@_...^-2)
}
```