[1]: https://rosettacode.org/wiki/Trigonometric_functions

# [Trigonometric functions][1]

 Borrow the degree to radian routine from [here](https://rosettacode.org/wiki/Length_of_an_arc_between_two_angles#Raku).

```perl
# 20210212 Updated Raku programming solution

sub postfix:<°>    (\ᵒ) { ᵒ × τ / 360 }

sub postfix:<㎭🡆°> (\ᶜ) { ᶜ / π × 180 }

say sin π/3 ;
say sin 60° ;

say cos π/4 ;
say cos 45° ;

say tan π/6 ;
say tan 30° ;

( asin(3.sqrt/2), acos(1/sqrt 2), atan(1/sqrt 3) )».&{ .say and .㎭🡆°.say }
```

#### Output:
```
0.8660254037844386
0.8660254037844386
0.7071067811865476
0.7071067811865476
0.5773502691896257
0.5773502691896257
1.0471975511965976
60
0.7853981633974484
45.00000000000001
0.5235987755982989
30.000000000000004
```
