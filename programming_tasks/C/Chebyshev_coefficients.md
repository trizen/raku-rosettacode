[1]: https://rosettacode.org/wiki/Chebyshev_coefficients

# [Chebyshev coefficients][1]



```perl
sub chebft ( Code $func, Real \a, Real \b, Int \n ) {

    my \bma = ½ × (b - a);
    my \bpa = ½ × (b + a);

    my @pi-n = ( ^n »+» ½ ) »×» (π/n);
    my @f    = ( @pi-n».cos »×» bma »+» bpa )».&$func;
    my @sums = (^n).map: { [+] @f »×« ( @pi-n »×» $_ )».cos };

    @sums »×» (2/n)
}

say chebft(&cos, 0, 1, 10)».fmt: '%+13.7e';
```

#### Output:
```
+1.6471695e+00
-2.3229937e-01
-5.3715115e-02
+2.4582353e-03
+2.8211906e-04
-7.7222292e-06
-5.8985565e-07
+1.1521427e-08
+6.5962992e-10
-1.0021994e-11
```
