[1]: https://rosettacode.org/wiki/CRC-32

# [CRC-32][1]





### Call to native function crc32 in zlib

```perl
use NativeCall;
 
sub crc32(int32 $crc, Buf $buf, int32 $len --> int32) is native('z') { * }
 
my $buf = 'The quick brown fox jumps over the lazy dog'.encode;
say crc32(0, $buf, $buf.bytes).fmt('%08x');
```


The libary name "z" resolves to `/usr/lib/libz.so` on a typical Linux system and `/usr/lib/libz.dylib` on Mac OS X, but may need to be changed for other platforms. Types may be platform-dependent as well. As written, the solution has been tested on Mac OS X 10.5.8 and Arch Linux 2016.08.01 x86\_64.


```
414fa339
```


### Pure Raku



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
    my @bit = flat ($buf.list X+& (1 X+< @bitorder))».so».Int, 0 xx $n;

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
