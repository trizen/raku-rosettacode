[1]: https://rosettacode.org/wiki/Length_of_an_arc_between_two_angles

# [Length of an arc between two angles][1]

```raku
sub arc ( \r, \a1, \a2 ) { r × (τ - abs(a2 - a1)) }
sub postfix:<°> (\d) { d × τ / 360 }
 
say arc(10, 10°, 120°), ' engineering units';
```

#### Output:
```
43.63323129985824 engineering units
```
