[1]: https://rosettacode.org/wiki/Main_step_of_GOST_28147-89

# [Main step of GOST 28147-89][1]





Implemented to match explanation on Discussion page:

```perl
# sboxes from https://en.wikipedia.org/wiki/GOST_(block_cipher)
constant sbox =
    [4, 10, 9, 2, 13, 8, 0, 14, 6, 11, 1, 12, 7, 15, 5, 3],
    [14, 11, 4, 12, 6, 13, 15, 10, 2, 3, 8, 1, 0, 7, 5, 9],
    [5, 8, 1, 13, 10, 3, 4, 2, 14, 15, 12, 7, 6, 0, 9, 11],
    [7, 13, 10, 1, 0, 8, 9, 15, 14, 4, 6, 12, 11, 2, 5, 3],
    [6, 12, 7, 1, 5, 15, 13, 8, 4, 10, 9, 14, 0, 3, 11, 2],
    [4, 11, 10, 0, 7, 2, 1, 13, 3, 6, 8, 5, 9, 12, 15, 14],
    [13, 11, 4, 1, 3, 15, 5, 9, 0, 10, 14, 7, 6, 8, 2, 12],
    [1, 15, 13, 0, 5, 7, 10, 4, 9, 2, 3, 14, 6, 11, 8, 12];
 
sub infix:<rol³²>(\y, \n) { (y +< n) % 2**32 +| (y +> (32 - n)) }
 
sub ГОСТ-round(\R, \K) {
    my \a = (R + K) % 2**32;
    my \b = :16[ sbox[$_][(a +> (4 * $_)) % 16] for 7...0 ];
    b rol³² 11;
}
 
sub feistel-step(&F, \L, \R, \K) { R, L +^ F(R, K) }
 
my @input = 0x21, 0x04, 0x3B, 0x04, 0x30, 0x04, 0x32, 0x04;
my @key   = 0xF9, 0x04, 0xC1, 0xE2;
 
my ($L,$R) = @input.reverse.map: { :256[$^a,$^b,$^c,$^d] }
my ($K   ) = @key  .reverse.map: { :256[$^a,$^b,$^c,$^d] }
 
($L,$R) = feistel-step(&ГОСТ-round, $L, $R, $K);

say [ ($L +< 32 + $R X+> (0, 8 ... 56)) X% 256 ].fmt('%02X');
```

#### Output:
```
1F 88 CF 07 21 04 3B 04
```
