[1]: https://rosettacode.org/wiki/Terminal_control/Display_an_extended_character

# [Terminal control/Display an extended character][1]


To demonstrate we're not limited to Latin-1, we'll print the fullwidth variant.

```perl
say '￡';
say "\x[FFE1]";
say "\c[FULLWIDTH POUND SIGN]";
0xffe1.chr.say;
```
