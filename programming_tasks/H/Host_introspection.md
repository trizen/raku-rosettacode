[1]: https://rosettacode.org/wiki/Host_introspection

# [Host introspection][1]

```raku
use experimental :pack;
say $*VM.config<ptr_size>;
say pack('N', 123456789).unpack('V') == 123456789 ?? 'big-endian' !! 'little-endian';
```

#### Output:
```
8
little-endian
```