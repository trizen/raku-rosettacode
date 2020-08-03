[1]: https://rosettacode.org/wiki/Gradient_descent

# [Gradient descent][1]

```raku
#!/usr/bin/env perl6
 
use v6.d;
 
sub steepestDescent(@x, $alpha is copy, $h is copy) {
 
   my $g0 = g(@x) ;    # Initial estimate of result.
 
   my @fi = gradG(@x, $h, $g0) ;    #  Calculate initial gradient
 
   # Calculate initial norm.
   my $b = $alpha / sqrt(my $delG = sum(map {$_²}, @fi));
 
   while ( $delG > $h ) {   # Iterate until value is <= tolerance.
 
      for @fi.kv -> $i, $j { @x[$i] -= $b * $j } #  Calculate next value.
 
      # Calculate next gradient and next value
      @fi = gradG(@x, $h /= 2, my $g1 = g(@x));  
 
      $b = $alpha / sqrt($delG = sum(map {$_²}, @fi) );  # Calculate next norm.
 
      $g1 > $g0 ?? ( $alpha /= 2 ) !! ( $g0 = $g1 )   # Adjust parameter.
   }
}
 
sub gradG(@x is copy, $h, $g0) { # gives a rough calculation of gradient g(x).
   return map { $_ += $h ; (g(@x) - $g0) / $h }, @x
}
 
# Function for which minimum is to be found.
sub g(\x) { (x[0]-1)² * exp(-x[1]²) + x[1]*(x[1]+2) * exp(-2*x[0]²) }
 
 
my $tolerance = 0.0000006 ; my $alpha = 0.1;
 
my @x = 0.1, -1; # Initial guess of location of minimum.
 
steepestDescent(@x, $alpha, $tolerance);
 
say "Testing steepest descent method:";
say "The minimum is at x[0] = ", @x[0], ", x[1] = ", @x[1];
 
```

#### Output:
```
Testing steepest descent method:
The minimum is at x[0] = 0.10743450794656964, x[1] = -1.2233956711774543
```
