[1]: https://rosettacode.org/wiki/Averages/Mean_angle

# [Averages/Mean angle][1]

This solution refuses to return an answer when the angles cancel out to a tiny magnitude.

```raku
# Of course, you can still use pi and 180.
sub deg2rad { $^d * tau / 360 }
sub rad2deg { $^r * 360 / tau }
 
sub phase ($c)  {
    my ($mag,$ang) = $c.polar;
    return NaN if $mag < 1e-16;
    $ang;
}
 
sub meanAngle { rad2deg phase [+] map { cis deg2rad $_ }, @^angles }
 
say meanAngle($_).fmt("%.2f\tis the mean angle of "), $_ for
    [350, 10],
    [90, 180, 270, 360],
    [10, 20, 30];
```

#### Output:
```
-0.00   is the mean angle of 350 10
NaN     is the mean angle of 90 180 270 360
20.00   is the mean angle of 10 20 30
```