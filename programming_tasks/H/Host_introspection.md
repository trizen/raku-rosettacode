[1]: https://rosettacode.org/wiki/Host_introspection

# [Host introspection][1]



```perl
use NativeCall;
say $*VM.config<ptr_size>;
my $bytes = nativecast(CArray[uint8], CArray[uint16].new(1));
say $bytes[0] ?? "little-endian" !! "big-endian";
```

#### Output:
```
8
little-endian
```


Note: Rakudo 2018.12 is introducing the endian-sensitive`read-int16` method,
which makes endian detection a little easier:

```perl
say blob8.new(1,0).read-int16(0) == 1 ?? "little-endian" !! "big-endian"
```


In Rakudo 2019.01 the dynamic KERNEL variable was fleshed out with a bunch of accessors, among them:

```perl
say join ', ', $*KERNEL, $*KERNEL.bits, $*KERNEL.arch, $*KERNEL.endian
```

#### Output:
```
linux, 64, x86_64, LittleEndian
```
