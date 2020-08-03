[1]: https://rosettacode.org/wiki/Terminal_control/Clear_the_screen

# [Terminal control/Clear the screen][1]

```raku
sub clear { print state $ = qx[clear] }
clear;
```