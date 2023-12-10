[1]: https://rosettacode.org/wiki/Find_the_intersection_of_two_lines

# [Find the intersection of two lines][1]



```perl
sub intersection (Real $ax, Real $ay, Real $bx, Real $by,
                  Real $cx, Real $cy, Real $dx, Real $dy ) {

    sub term:<|AB|> { determinate($ax, $ay, $bx, $by) }
    sub term:<|CD|> { determinate($cx, $cy, $dx, $dy) }

    my $ΔxAB = $ax - $bx;
    my $ΔyAB = $ay - $by;
    my $ΔxCD = $cx - $dx;
    my $ΔyCD = $cy - $dy;

    my $x-numerator = determinate( |AB|, $ΔxAB, |CD|, $ΔxCD );
    my $y-numerator = determinate( |AB|, $ΔyAB, |CD|, $ΔyCD );
    my $denominator = determinate( $ΔxAB, $ΔyAB, $ΔxCD, $ΔyCD );

    return 'Lines are parallel' if $denominator == 0;

    ($x-numerator/$denominator, $y-numerator/$denominator);
}

sub determinate ( Real $a, Real $b, Real $c, Real $d ) { $a * $d - $b * $c }

# TESTING
say 'Intersection point: ', intersection( 4,0, 6,10, 0,3, 10,7 );
say 'Intersection point: ', intersection( 4,0, 6,10, 0,3, 10,7.1 );
say 'Intersection point: ', intersection( 0,0, 1,1, 1,2, 4,5 );
```

#### Output:
```
Intersection point: (5 5)
Intersection point: (5.010893 5.054466)
Intersection point: Lines are parallel
```


### Using a geometric algebra library



See task [geometric algebra](https://rosettacode.org/wiki/Geometric_algebra).

```perl
use Clifford;

# We pick a projective basis,
# and we compute its pseudo-scalar and its square.
my ($i, $j, $k) = @e;
my $I = $i∧$j∧$k;
my $I2 = ($I**2).narrow;

# Homogeneous coordinates of point (X,Y) are (X,Y,1)
my $A =  4*$i +  0*$j + $k;
my $B =  6*$i + 10*$j + $k;
my $C =  0*$i +  3*$j + $k;
my $D = 10*$i +  7*$j + $k;

# We form lines by joining points
my $AB = $A∧$B;
my $CD = $C∧$D;

# The intersection is their meet, which we
# compute by using the De Morgan law
my $ab = $AB*$I/$I2;
my $cd = $CD*$I/$I2;
my $M = ($ab ∧ $cd)*$I/$I2;

# Affine coordinates are (X/Z, Y/Z)
say $M/($M·$k) X· $i, $j;
```

#### Output:
```
(5 5)
```
