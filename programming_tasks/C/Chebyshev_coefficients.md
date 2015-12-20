[1]: http://rosettacode.org/wiki/Chebyshev_coefficients

# [Chebyshev coefficients][1]

```perl6
sub chebft ( Code $func, Real $a, Real $b, Int $n ) {
 
    my $bma = 0.5 * ( $b - $a );
    my $bpa = 0.5 * ( $b + $a );
 
    my @pi_n = ( (^$n).list »+» 0.5 ) »*» ( pi / $n );
    my @f    = ( @pi_n».cos »*» $bma »+» $bpa )».$func;
    my @sums = map { [+] @f »*« ( @pi_n »*» $_ )».cos }, ^$n;
 
    return @sums »*» ( 2 / $n );
}
 
say .fmt('%+13.7e') for chebft &cos, 0, 1, 10;
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