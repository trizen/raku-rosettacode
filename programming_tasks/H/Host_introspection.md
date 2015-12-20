[1]: http://rosettacode.org/wiki/Host_introspection

# [Host introspection][1]

```perl6
say $*VM.config<ptr_size>;
say pack('N', 123456789).unpack('V') == 123456789 ?? 'big-endian' !! 'little-endian';
```

#### Output:
```
8
little-endian
```