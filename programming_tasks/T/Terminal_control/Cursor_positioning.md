[1]: https://rosettacode.org/wiki/Terminal_control/Cursor_positioning

# [Terminal control/Cursor positioning][1]

Assuming an ANSI terminal:

```raku
print "\e[6;3H";
print 'Hello';
```