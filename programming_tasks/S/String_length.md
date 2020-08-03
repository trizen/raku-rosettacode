[1]: https://rosettacode.org/wiki/String_length

# [String length][1]

### Byte Length

```perl
say 'møøse'.encode('UTF-8').bytes;
```


### Character Length

```perl
say 'møøse'.codes;
```


### Grapheme Length

```perl
say 'møøse'.chars;
```