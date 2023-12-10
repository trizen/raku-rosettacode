[1]: https://rosettacode.org/wiki/Find_the_intersection_of_a_line_with_a_plane

# [Find the intersection of a line with a plane][1]



```perl
class Line {
    has $.P0; # point
    has $.u⃗;  # ray
}
class Plane {
    has $.V0; # point
    has $.n⃗;  # normal
}

sub infix:<∙> ( @a, @b where +@a == +@b ) { [+] @a «*» @b } # dot product

sub line-plane-intersection ($𝑳, $𝑷) {
    my $cos = $𝑷.n⃗ ∙ $𝑳.u⃗; # cosine between normal & ray
    return 'Vectors are orthogonal; no intersection or line within plane'
      if $cos == 0;
    my $𝑊 = $𝑳.P0 «-» $𝑷.V0;      # difference between P0 and V0
    my $S𝐼 = -($𝑷.n⃗ ∙ $𝑊) / $cos;  # line segment where it intersects the plane
    $𝑊 «+» $S𝐼 «*» $𝑳.u⃗ «+» $𝑷.V0; # point where line intersects the plane
 }

say 'Intersection at point: ', line-plane-intersection(
     Line.new( :P0(0,0,10), :u⃗(0,-1,-1) ),
    Plane.new( :V0(0,0, 5), :n⃗(0, 0, 1) )
  );
```

#### Output:
```
Intersection at point: (0 -5 5)
```


### With a geometric algebra library



See task [geometric algebra](https://rosettacode.org/wiki/Geometric_algebra)

```perl
use Clifford:ver<6.2.1>;

# We pick a (non-degenerate) projective basis and
# we define the dual and meet operators.
my $I = [∧] my ($i, $j, $k, $l) = @e;
sub prefix:<∗>($M) { $M/$I }
sub infix:<∨>($A, $B) { ∗((∗$B)∧(∗$A)) }

my $direction = -$j - $k;

# Homogeneous coordinates of (X, Y, Z) are (X, Y, Z, 1)
my $point = 10*$k + $l;

# A projective line is a bivector
my $line = $direction ∧ $point;

# A projective plane is a trivector
my $plane = (5*$k + $l) ∧ ($k*-$i∧$j∧$k);

# The intersection is the meet
my $m = $line ∨ $plane;

# Affine coordinates of (X, Y, Z, W) are (X/W, Y/W, Z/W)
say $m/($m·$l) X· ($i, $j, $k);
```

#### Output:
```
(0 -5 5)
```
