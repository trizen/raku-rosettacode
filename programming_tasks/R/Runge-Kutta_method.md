[1]: http://rosettacode.org/wiki/Runge-Kutta_method

# [Runge-Kutta method][1]

```perl6
sub runge-kutta(&yp) {
    return -> \t, \y, \δt {
        my $a = δt * yp( t, y );
        my $b = δt * yp( t + δt/2, y + $a/2 );
        my $c = δt * yp( t + δt/2, y + $b/2 );
        my $d = δt * yp( t + δt, y + $c );
        ($a + 2*($b + $c) + $d) / 6;
    }
}
 
constant δt = .1;
my &δy = runge-kutta { $^t * sqrt($^y) };
 
loop (
    my ($t, $y) = (0, 1);
    $t <= 10;
    $t, $y Z[+=] δt, δy($t, $y, δt)
) {
    printf "y(%2d) = %12f ± %e\n", $t, $y, abs($y - ($t**2 + 4)**2 / 16)
    if $t.narrow ~~ Int;
}
```

#### Output:
```
y( 0) =     1.000000 ± 0.000000e+00
y( 1) =     1.562500 ± 1.457219e-07
y( 2) =     3.999999 ± 9.194792e-07
y( 3) =    10.562497 ± 2.909562e-06
y( 4) =    24.999994 ± 6.234909e-06
y( 5) =    52.562489 ± 1.081970e-05
y( 6) =    99.999983 ± 1.659460e-05
y( 7) =   175.562476 ± 2.351773e-05
y( 8) =   288.999968 ± 3.156520e-05
y( 9) =   451.562459 ± 4.072316e-05
y(10) =   675.999949 ± 5.098329e-05
```