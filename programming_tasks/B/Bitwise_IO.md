[1]: https://rosettacode.org/wiki/Bitwise_IO

# [Bitwise IO][1]



```perl
sub encode-ascii(Str $s) {
    my @b = flat $s.ords».fmt("%07b")».comb;
    @b.push(0) until @b %% 8;   # padding
    Buf.new: gather while @b { take reduce * *2+*, (@b.pop for ^8) }
}

sub decode-ascii(Buf $b) {
    my @b = flat $b.list».fmt("%08b")».comb;
    @b.shift until @b %% 7;   # remove padding
    @b = gather while @b { take reduce * *2+*, (@b.pop for ^7) }
    return [~] @b».chr;
}
say my $encode = encode-ascii 'STRING';
say decode-ascii $encode;
```

#### Output:
```
Buf:0x<03 8b 99 29 4a e5>
STRING
```
