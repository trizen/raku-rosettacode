[1]: http://rosettacode.org/wiki/Euler_method

# [Euler method][1]

```perl6
sub euler ( &f, $y0, $a, $b, $h ) {
    my $y = $y0;
    my @t_y;
    for $a, * + $h ... * > $b -> $t {
        @t_y[$t] = $y;
        $y += $h * f( $t, $y );
    }
    return @t_y;
}
 
constant COOLING_RATE = 0.07;
constant AMBIENT_TEMP =   20;
constant INITIAL_TEMP =  100;
constant INITIAL_TIME =    0;
constant FINAL_TIME   =  100;
 
sub f ( $time, $temp ) {
    return -COOLING_RATE * ( $temp - AMBIENT_TEMP );
}
 
my @e;
@e[$_] = euler( &f, INITIAL_TEMP, INITIAL_TIME, FINAL_TIME, $_ ) for 2, 5, 10;
 
say 'Time Analytic   Step2   Step5  Step10     Err2     Err5    Err10';
 
for INITIAL_TIME, * + 10 ... * >= FINAL_TIME -> $t {
 
    my $exact = AMBIENT_TEMP + (INITIAL_TEMP - AMBIENT_TEMP)
                              * (-COOLING_RATE * $t).exp;
 
    my $err = sub { @^a.map: { 100 * abs( $_ - $exact ) / $exact } }
 
    my ( $a, $b, $c ) = map { @e[$_][$t] }, 2, 5, 10;
 
    say $t.fmt('%4d '), ( $exact, $a, $b, $c )».fmt(' %7.3f'),
                           $err.([$a, $b, $c])».fmt(' %7.3f%%');
}
```

#### Output:
```
Time Analytic   Step2   Step5  Step10     Err2     Err5    Err10
   0  100.000 100.000 100.000 100.000   0.000%   0.000%   0.000%
  10   59.727  57.634  53.800  44.000   3.504%   9.923%  26.331%
  20   39.728  37.704  34.281  27.200   5.094%  13.711%  31.534%
  30   29.797  28.328  26.034  22.160   4.927%  12.629%  25.629%
  40   24.865  23.918  22.549  20.648   3.808%   9.313%  16.959%
  50   22.416  21.843  21.077  20.194   2.555%   5.972%   9.910%
  60   21.200  20.867  20.455  20.058   1.569%   3.512%   5.384%
  70   20.596  20.408  20.192  20.017   0.912%   1.959%   2.808%
  80   20.296  20.192  20.081  20.005   0.512%   1.057%   1.432%
  90   20.147  20.090  20.034  20.002   0.281%   0.559%   0.721%
 100   20.073  20.042  20.014  20.000   0.152%   0.291%   0.361%
```