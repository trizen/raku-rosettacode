[1]: https://rosettacode.org/wiki/Length_of_an_arc_between_two_angles

# [Length of an arc between two angles][1]

Taking a slightly different approach. Rather than the simplest thing that
could possibly work, implements a reusable arc-length routine. Standard notation
for angles has the zero to the right along an 'x' axis with a
counter-clockwise  rotation for increasing angles. This version follows
convention and assumes the  first given angle is "before" the second when
rotating counter-clockwise. In order to return the major swept angle in the
task example, you need to supply the "second" angle first. (The measurement
will be from the first given angle counter-clockwise to the second.)



If you don't supply a radius, returns the radian arc angle which may then be
multiplied by the radius to get actual circumferential length.



Works in radian angles by default but provides a postfix ° operator to convert
degrees to radians and a postfix ᵍ to convert gradians to radians.

```perl
sub arc ( Real \a1, Real \a2, :r(:$radius) = 1 ) {
    ( ([-] (a2, a1).map((* + τ) % τ)) + τ ) % τ × $radius
}

sub postfix:<°> (\d) { d × τ / 360 }
sub postfix:<ᵍ> (\g) { g × τ / 400 }

say 'Task example: from 120° counter-clockwise to 10° with 10 unit radius';
say arc(:10radius, 120°, 10°), ' engineering units';

say "\nSome test examples:";
for \(120°, 10°), # radian magnitude (unit radius)
    \(10°, 120°), # radian magnitude (unit radius)
    \(:radius(10/π), 180°, -90°), # 20 unit circumference for ease of comparison
    \(0°, -90°, :r(10/π),),       #  ↓  ↓  ↓  ↓  ↓  ↓  ↓
    \(:radius(10/π), 0°, 90°),
    \(π/4, 7*π/4, :r(10/π)),
    \(175ᵍ, -45ᵍ, :r(10/π)) {  # test gradian parameters
    printf "Arc length: %8s  Parameters: %s\n", arc(|$_).round(.000001), $_.raku
}
```

#### Output:
```
Task example: from 120° counter-clockwise to 10° with 10 unit radius
43.63323129985824 engineering units

Some test examples:
Arc length: 4.363323  Parameters: \(2.0943951023931953e0, 0.17453292519943295e0)
Arc length: 1.919862  Parameters: \(0.17453292519943295e0, 2.0943951023931953e0)
Arc length:        5  Parameters: \(3.141592653589793e0, -1.5707963267948966e0, :radius(3.183098861837907e0))
Arc length:       15  Parameters: \(0e0, -1.5707963267948966e0, :r(3.183098861837907e0))
Arc length:        5  Parameters: \(0e0, 1.5707963267948966e0, :radius(3.183098861837907e0))
Arc length:       15  Parameters: \(0.7853981633974483e0, 5.497787143782138e0, :r(3.183098861837907e0))
Arc length:        9  Parameters: \(2.7488935718910685e0, -0.7068583470577035e0, :r(3.183098861837907e0))
```
