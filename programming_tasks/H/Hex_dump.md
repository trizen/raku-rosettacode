[1]: https://rosettacode.org/wiki/Hex_dump

# [Hex dump][1]

The hexdump() routine expects a Blob. How you feed it the Blob is up to you. You may read a file in binary mode, encode a string, whatever. Using encoded strings here for a compact, self-contained example. Encoded in a few different ways for variety.

```perl
say my $string = 'Rosettacode is a programming crestomathy site 😀.    🍨 👨‍👩‍👦 ⚽ ¯\_(ツ)_/¯';

say "\nHex dump UTF-16BE, offset 0";
hexdump $string.encode("UTF-16BE");

say "\nHex dump UTF-16LE, offset 0";
hexdump $string.encode("UTF-16LE");

say "\nBinary dump UTF-8, offset 17";
hexdump $string.encode("UTF-8"), :bin, :17offset;

sub hexdump(Blob $blob, Int :$offset is copy = 0, :$bin = False) {
    my ($fmt, $space, $batch) = $bin ?? ("%08b", ' ' x 8, 6) !! ("%02X", '  ', 16);
    for $blob.skip($offset).batch($batch) {
        my @h = flat $_».fmt($fmt), $space xx $batch;
        @h[7] ~= ' ';
        printf "%08X  %s  |%s|\n", $offset, @h[^$batch].Str,
           $_».chr.join.subst(/<-print>|\t|\n/, '.', :g);
        $offset += $batch;
    }
}
```

#### Output:
```
Rosettacode is a programming crestomathy site 😀.    🍨 👨‍👩‍👦 ⚽ ¯\_(ツ)_/¯

Hex dump UTF-16BE, offset 0
00000000  00 52 00 6F 00 73 00 65  00 74 00 74 00 61 00 63  |.R.o.s.e.t.t.a.c|
00000010  00 6F 00 64 00 65 00 20  00 69 00 73 00 20 00 61  |.o.d.e. .i.s. .a|
00000020  00 20 00 70 00 72 00 6F  00 67 00 72 00 61 00 6D  |. .p.r.o.g.r.a.m|
00000030  00 6D 00 69 00 6E 00 67  00 20 00 63 00 72 00 65  |.m.i.n.g. .c.r.e|
00000040  00 73 00 74 00 6F 00 6D  00 61 00 74 00 68 00 79  |.s.t.o.m.a.t.h.y|
00000050  00 20 00 73 00 69 00 74  00 65 00 20 D8 3D DE 00  |. .s.i.t.e. Ø=Þ.|
00000060  00 2E 00 20 00 20 00 20  00 20 D8 3C DF 68 00 20  |... . . . Ø<ßh. |
00000070  D8 3D DC 68 20 0D D8 3D  DC 69 20 0D D8 3D DC 66  |Ø=Üh .Ø=Üi .Ø=Üf|
00000080  00 20 26 BD 00 20 00 AF  00 5C 00 5F 00 28 30 C4  |. &½. .¯.\._.(0Ä|
00000090  00 29 00 5F 00 2F 00 AF                           |.)._./.¯|

Hex dump UTF-16LE, offset 0
00000000  52 00 6F 00 73 00 65 00  74 00 74 00 61 00 63 00  |R.o.s.e.t.t.a.c.|
00000010  6F 00 64 00 65 00 20 00  69 00 73 00 20 00 61 00  |o.d.e. .i.s. .a.|
00000020  20 00 70 00 72 00 6F 00  67 00 72 00 61 00 6D 00  | .p.r.o.g.r.a.m.|
00000030  6D 00 69 00 6E 00 67 00  20 00 63 00 72 00 65 00  |m.i.n.g. .c.r.e.|
00000040  73 00 74 00 6F 00 6D 00  61 00 74 00 68 00 79 00  |s.t.o.m.a.t.h.y.|
00000050  20 00 73 00 69 00 74 00  65 00 20 00 3D D8 00 DE  | .s.i.t.e. .=Ø.Þ|
00000060  2E 00 20 00 20 00 20 00  20 00 3C D8 68 DF 20 00  |.. . . . .<Øhß .|
00000070  3D D8 68 DC 0D 20 3D D8  69 DC 0D 20 3D D8 66 DC  |=ØhÜ. =ØiÜ. =ØfÜ|
00000080  20 00 BD 26 20 00 AF 00  5C 00 5F 00 28 00 C4 30  | .½& .¯.\._.(.Ä0|
00000090  29 00 5F 00 2F 00 AF 00                           |)._./.¯.|

Binary dump UTF-8, offset 17
00000011  01110000 01110010 01101111 01100111 01110010 01100001  |progra|
00000017  01101101 01101101 01101001 01101110 01100111 00100000  |mming |
0000001D  01100011 01110010 01100101 01110011 01110100 01101111  |cresto|
00000023  01101101 01100001 01110100 01101000 01111001 00100000  |mathy |
00000029  01110011 01101001 01110100 01100101 00100000 11110000  |site ð|
0000002F  10011111 10011000 10000000 00101110 00100000 00100000  |....  |
00000035  00100000 00100000 11110000 10011111 10001101 10101000  |  ð..¨|
0000003B  00100000 11110000 10011111 10010001 10101000 11100010  | ð..¨â|
00000041  10000000 10001101 11110000 10011111 10010001 10101001  |..ð..©|
00000047  11100010 10000000 10001101 11110000 10011111 10010001  |â..ð..|
0000004D  10100110 00100000 11100010 10011010 10111101 00100000  |¦ â.½ |
00000053  11000010 10101111 01011100 01011111 00101000 11100011  |Â¯\_(ã|
00000059  10000011 10000100 00101001 01011111 00101111 11000010  |..)_/Â|
0000005F  10101111                                               |¯|
```
