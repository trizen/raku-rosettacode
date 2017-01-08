[1]: http://rosettacode.org/wiki/Ramer-Douglas-Peucker_line_simplification

# [Ramer-Douglas-Peucker line simplification][1]

```perl
sub perpendicular-distance (@start, @end where @end !eqv @start , @point) {
    return 0 if @point eqv any(@start, @end);
    my ($Δx,  $Δy ) =   @end «-» @start;
    my ($Δpx, $Δpy) = @point «-» @start;
    ($Δx, $Δy) «/=» ($Δx² + $Δy²).sqrt;
    ((($Δpx, $Δpy) «-» ($Δx, $Δy) «*» ($Δx*$Δpx + $Δy*$Δpy)) «**» 2).sum.sqrt;
}
 
sub Ramer-Douglas-Peucker( @points where * > 1, \ε = 1 ) {
    return @points if @points == 2;
    my @d = (^@points).map:
       { perpendicular-distance(@points[0], @points[*-1], @points[$_]) };
    my ($index, $dmax) = @d.first: @d.max, :kv;
 
    if $dmax > ε {
        return Ramer-Douglas-Peucker( @points[0..$index]  , ε )[0..*-2],
               Ramer-Douglas-Peucker( @points[$index..*-1], ε )
    } else {
        return @points[0, *-1];
    }
}
# TESTING
say flat Ramer-Douglas-Peucker(
   [(0,0),(1,0.1),(2,-0.1),(3,5),(4,6),(5,7),(6,8.1),(7,9),(8,9),(9,9)]
);
```

#### Output:
```
((0 0) (2 -0.1) (3 5) (7 9) (9 9))
```