[1]: http://rosettacode.org/wiki/Character_codes

# [Character codes][1]

Both Perl 5 and Perl 6 have good Unicode support. Note that even characters outside the BMP are considered single characters, not a surrogate pair. Here we use the character "four dragons" (with 64 strokes!) to demonstrate that.

```perl6
say ord('𪚥').fmt('0x%04x');
say chr(0x2a6a5);
```

#### Output:
```
0x2a6a5
</dt></dl>
𪚥
```