[1]: https://rosettacode.org/wiki/String_length

# [String length][1]

### Byte Length

```raku
say 'møøse'.encode('UTF-8').bytes;
```


### Character Length

```raku
say 'møøse'.codes;
```


### Grapheme Length

```raku
say 'møøse'.chars;
```