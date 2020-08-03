[1]: https://rosettacode.org/wiki/Mouse_position

# [Mouse position][1]

```raku
use java::awt::MouseInfo:from<java>;
 
given MouseInfo.getPointerInfo.getLocation {
    say .getX, 'x', .getY;
}
```