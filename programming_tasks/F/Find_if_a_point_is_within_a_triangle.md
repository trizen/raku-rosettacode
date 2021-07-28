[1]: https://rosettacode.org/wiki/Find_if_a_point_is_within_a_triangle

# [Find if a point is within a triangle][1]

Reusing code from the [Convex hull](https://rosettacode.org/wiki/Convex_hull#Raku) task and some logic from the [Determine if two triangles overlap](https://rosettacode.org/wiki/Determine_if_two_triangles_overlap#Raku) task.

```perl
class Point {
    has Real $.x is rw;
    has Real $.y is rw;
    method gist { [~] '(', self.x,', ', self.y, ')' };
}
 
sub sign (Point $a, Point $b, Point $c) {
    ($b.x - $a.x)*($c.y - $a.y) - ($b.y - $a.y)*($c.x - $a.x);
}
 
sub triangle (*@points where *.elems == 6) {
    @points.batch(2).map: { Point.new(:x(.[0]),:y(.[1])) };
}
 
sub is-within ($point, @triangle is copy) {
   my @signs = sign($point, |(@triangle.=rotate)[0,1]) xx 3;
   so (all(@signs) >= 0) or so(all(@signs) <= 0);
}
 
my @triangle = triangle((1.5, 2.4), (5.1, -3.1), (-3.8, 0.5));
 
for Point.new(:x(0),:y(0)),
    Point.new(:x(0),:y(1)),
    Point.new(:x(3),:y(1))
  -> $point {
    say "Point {$point.gist} is within triangle {join ', ', @triangle».gist}: ",
        $point.&is-within: @triangle
}
```

#### Output:
```
Point (0, 0) is within triangle (1.5, 2.4), (5.1, -3.1), (-3.8, 0.5): True
Point (0, 1) is within triangle (1.5, 2.4), (5.1, -3.1), (-3.8, 0.5): True
Point (3, 1) is within triangle (1.5, 2.4), (5.1, -3.1), (-3.8, 0.5): False
```
