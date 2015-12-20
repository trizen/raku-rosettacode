[1]: http://rosettacode.org/wiki/Mouse_position

# [Mouse position][1]

```perl6
use java::awt::MouseInfo:from<java>;
 
given MouseInfo.getPointerInfo.getLocation {
    say .getX, 'x', .getY;
}
```