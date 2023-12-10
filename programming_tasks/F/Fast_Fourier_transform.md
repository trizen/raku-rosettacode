[1]: https://rosettacode.org/wiki/Fast_Fourier_transform

# [Fast Fourier transform][1]



```perl
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
1-2.414213562373095i
0+0i
1-0.4142135623730949i
0+0i
0.9999999999999999+0.4142135623730949i
0+0i
0.9999999999999997+2.414213562373095i
```


For the fun of it, here is a purely functional version:

```perl
sub fft {
    @_ == 1 ?? @_ !!
    fft(@_[0,2...*]) «+«
    fft(@_[1,3...*]) «*« map &cis, (0,-τ/@_...^-τ)
}
```
