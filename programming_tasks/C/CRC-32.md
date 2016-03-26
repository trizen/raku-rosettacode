[1]: http://rosettacode.org/wiki/CRC-32

# [CRC-32][1]

Library name and types are platform-dependent. As written the solution has been tested on Mac OS X 10.5.8.



Note: Buf $buf would be preferable, but NativeCall does not support Buf parameters, yet.

```perl
use NativeCall;
 
sub crc32(int32 $crc, Str $buf, int32 $len --> int32) is native('/usr/lib/libz.dylib') { * }
 
my $buf = 'The quick brown fox jumps over the lazy dog';
say crc32(0, $buf, $buf.chars).fmt('%08x');
```

#### Output:
```
414fa339
```


A fairly generic implementation with no regard to execution speed:

```perl
sub crc(
    Blob $buf,
             # polynomial including leading term, default: ISO 3309/PNG/gzip
    :@poly = (1,0,0,0,0,0,1,0,0,1,1,0,0,0,0,0,1,0,0,0,1,1,1,0,1,1,0,1,1,0,1,1,1),
    :$n = @poly.end,      # degree of polynomial
    :@init = 1 xx $n,     # initial XOR bits
    :@fini = 1 xx $n,     # final XOR bits
    :@bitorder = 0..7,    # default: eat bytes LSB-first
    :@crcorder = 0..$n-1, # default: MSB of checksum is coefficient of x⁰
) {
    my @bit = flat ($buf.list X+& (1 X+< @bitorder).list)».so».Int, 0 xx $n;
 
    @bit[0   .. $n-1] «+^=» @init;
    @bit[$_  ..$_+$n] «+^=» @poly if @bit[$_] for 0..@bit.end-$n;
    @bit[*-$n..  *-1] «+^=» @fini;
 
    :2[@bit[@bit.end X- @crcorder]];
}
 
say crc('The quick brown fox jumps over the lazy dog'.encode('ascii')).base(16);
```

#### Output:
```
414FA339
```