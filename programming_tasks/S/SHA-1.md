[1]: https://rosettacode.org/wiki/SHA-1

# [SHA-1][1]

A pure Perl 6 implementation that closely follows the description of SHA-1 in FIPS 180-1. Slow.

```raku
sub postfix:<mod2³²> { $^x % 2**32 }
sub infix:<⊕>        { ($^x + $^y)mod2³² }
sub S                { ($^x +< $^n)mod2³² +| ($x +> (32-$n)) }
 
my \f = -> \B,\C,\D { (B +& C) +| ((+^B)mod2³² +& D)   },
        -> \B,\C,\D { B +^ C +^ D                      },
        -> \B,\C,\D { (B +& C) +| (B +& D) +| (C +& D) },
        -> \B,\C,\D { B +^ C +^ D                      };
 
my \K = 0x5A827999, 0x6ED9EBA1, 0x8F1BBCDC, 0xCA62C1D6;
 
sub sha1-pad(Blob $msg)
{   
    my \bits = 8 * $msg.elems;
    my @padded = flat $msg.list, 0x80, 0x00 xx (-($msg.elems + 1 + 8) % 64);
    flat @padded.map({ :256[$^a,$^b,$^c,$^d] }), (bits +> 32)mod2³², (bits)mod2³²;
}
 
sub sha1-block(@H, @M is copy)
{   
    @M.push: S(1, [+^] @M[$_ «-« <3 8 14 16>] ) for 16 .. 79;
 
    my ($A,$B,$C,$D,$E) = @H;
    for 0..79 -> \t {
        ($A, $B, $C, $D, $E) =
        S(5,$A) ⊕ f[t div 20]($B,$C,$D) ⊕ $E ⊕ @M[t] ⊕ K[t div 20],
        $A, S(30,$B), $C, $D;
    }
    @H »⊕=« ($A,$B,$C,$D,$E);
}
 
sub sha1(Blob $msg) returns Blob
{   
    my @M = sha1-pad($msg);
    my @H = 0x67452301, 0xEFCDAB89, 0x98BADCFE, 0x10325476, 0xC3D2E1F0;
    sha1-block(@H,@M[$_..$_+15]) for 0, 16...^ +@M;
    Blob.new: flat map { reverse .polymod(256 xx 3) }, @H;
}
 
say sha1(.encode('ascii')), "  $_"
   for 'abc',
       'abcdbcdecdefdefgefghfghighijhijkijkljklmklmnlmnomnopnopq',
       'Rosetta Code',
       'Ars longa, vita brevis';
```

#### Output:
```
Buf:0x<a9 99 3e 36 47 06 81 6a ba 3e 25 71 78 50 c2 6c 9c d0 d8 9d>  abc
Buf:0x<84 98 3e 44 1c 3b d2 6e ba ae 4a a1 f9 51 29 e5 e5 46 70 f1>  abcdbcdecdefdefgefghfghighijhijkijkljklmklmnlmnomnopnopq
Buf:0x<48 c9 8f 7e 5a 6e 73 6d 79 0a b7 40 df c3 f5 1a 61 ab e2 b5>  Rosetta Code
Buf:0x<e6 40 d2 85 24 28 86 eb 96 ab 80 cb f8 58 38 9b 3d f5 2f 43>  Ars longa, vita brevis
```