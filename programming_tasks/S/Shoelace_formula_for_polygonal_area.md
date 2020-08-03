[1]: https://rosettacode.org/wiki/Shoelace_formula_for_polygonal_area

# [Shoelace formula for polygonal area][1]

### Index and mod offset

```raku
sub area-by-shoelace(@p) {
    (^@p).map({@p[$_;0] * @p[($_+1)%@p;1] - @p[$_;1] * @p[($_+1)%@p;0]}).sum.abs / 2
}
 
say area-by-shoelace( [ (3,4), (5,11), (12,8), (9,5), (5,6) ] );
```

#### Output:
```
30
```


### Slice and rotation

```raku
sub area-by-shoelace ( @p ) {
    my @x := @p».[0];
    my @y := @p».[1];
 
    my $s := ( @x Z* @y.rotate( 1) ).sum
           - ( @x Z* @y.rotate(-1) ).sum;
 
    return $s.abs / 2;
}
 
say area-by-shoelace( [ (3,4), (5,11), (12,8), (9,5), (5,6) ] );
 
```

#### Output:
```
30
```