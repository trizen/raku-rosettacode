[1]: https://rosettacode.org/wiki/ASCII_control_characters

# [ASCII control characters][1]

There doesn't seem to really be a point or a purpose to this task other than creating a enumeration...



'space' is absolutely **NOT** a control character. Pretending it is is just completely incorrect.

```perl
enum C0 (|(^32).map({ (0x2400 + $_).chr => $_ }), '␡' => 127);

printf "Ord: %3d, Unicode: %s, Enum: %s\n", $_, .uniname, C0($_)
   for (^128).grep: {.chr ~~ /<:Cc>/}
```
