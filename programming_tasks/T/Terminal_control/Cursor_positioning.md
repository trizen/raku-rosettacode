[1]: http://rosettacode.org/wiki/Terminal_control/Cursor_positioning

# [Terminal control/Cursor positioning][1]

Assuming an ANSI terminal:

```perl6
print "\e[6;3H";
print 'Hello';
```