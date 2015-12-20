[1]: http://rosettacode.org/wiki/Return_multiple_values

# [Return multiple values][1]

Each function officially returns one value, but by returning a Parcel you can transparently return a lazy list of arbitrary size.

```perl
sub foo($a,$b) {
    $a + $b, $a * $b, $b xx $a
}
 
say foo 3, 7;
```

#### Output:
```
10 21 7 7 7
```