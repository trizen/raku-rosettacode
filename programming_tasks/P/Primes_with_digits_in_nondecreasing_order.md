[1]: https://rosettacode.org/wiki/Primes_with_digits_in_nondecreasing_order

# [Primes with digits in nondecreasing order][1]

```perl
my $range = ^1000;
 
for flat 2..10, 17, 27, 31 -> $base {
   say "\nBase $base: {+$_} non-decending primes between $range.minmax.map( *.base: $base ).join(' and '):\n{
      .batch(20)».fmt("%{.tail.chars}s").join: "\n" }"
       given $range.grep( *.is-prime ).map( *.base: $base ).grep: { [le] .comb }
}
```

#### Output:
```
Base 2: 4 non-decending primes between 0 and 1111100111:
     11     111   11111 1111111

Base 3: 6 non-decending primes between 0 and 1101000:
   2   12  111  122 1112 1222

Base 4: 17 non-decending primes between 0 and 33213:
    2     3    11    13    23   113   133   223   233  1223  1333  2333 11123 11233 11333 12233 22223

Base 5: 17 non-decending primes between 0 and 12444:
    2     3    12    23    34   111   122   133  1112  1123  1233  1244  2223  2344  3444 11122 12222

Base 6: 37 non-decending primes between 0 and 4343:
   2    3    5   11   15   25   35   45  111  115  125  135  155  225  245  255  335  345  445  455
1115 1125 1145 1235 1245 1335 1345 1355 1445 1555 2225 2335 2345 2555 3445 3455 3555

Base 7: 38 non-decending primes between 0 and 2625:
   2    3    5   14   16   23   25   56  113  115  124  133  146  155  166  245  256  335  344  346
 445  566 1112 1123 1136 1156 1222 1226 1235 1345 1444 1466 2234 2236 2333 2335 2366 2555

Base 8: 47 non-decending primes between 0 and 1747:
   2    3    5    7   13   15   23   27   35   37   45   57  111  117  123  145  147  155  177  225
 227  235  247  255  277  337  345  357  445  467  557  577  667 1113 1127 1137 1145 1167 1223 1225
1245 1335 1347 1357 1467 1555 1567

Base 9: 45 non-decending primes between 0 and 1330:
   2    3    5    7   12   14   18   25   34   45   47   58   67   78  117  122  124  128  135  155
 177  234  238  267  278  337  344  355  377  447  557  568  667  678  788 1112 1114 1118 1147 1158
1178 1222 1255 1268 1288

Base 10: 50 non-decending primes between 0 and 999:
  2   3   5   7  11  13  17  19  23  29  37  47  59  67  79  89 113 127 137 139
149 157 167 179 199 223 227 229 233 239 257 269 277 337 347 349 359 367 379 389
449 457 467 479 499 557 569 577 599 677

Base 17: 82 non-decending primes between 0 and 37D:
  2   3   5   7   B   D  12  16  1C  1E  23  27  29  2D  38  3A  3G  45  4B  4F
 5C  5G  67  6B  78  7C  8D  8F  9A  9E  AB  BC  FG 111 115 117 11B 128 12E 137
139 13D 14A 14G 155 159 15F 166 16A 17B 17D 188 18E 19F 1BB 1BF 1CG 1DD 1EE 1GG
225 227 23C 23E 247 24D 24F 25A 25E 26B 27C 28D 29C 2AD 2CF 33B 346 34C 35F 368
36E 37B

Base 27: 103 non-decending primes between 0 and 1A0:
  2   3   5   7   B   D   H   J   N  12  14  1A  1E  1G  1K  1Q  25  27  2D  2H
 2J  2P  38  3G  3K  3M  3Q  45  4J  4N  5E  5G  5M  6B  6H  6J  78  7A  7M  8B
 8D  8H  8N  8P  9E  9K  9Q  AB  AD  AN  BE  BG  BK  CD  CN  CP  DG  DM  EJ  EN
 FG  FQ  GH  GP  HK  IN  KN  LQ  MN  MP  NQ  OP  PQ 111 115 11D 11H 124 12E 12Q
13B 13D 13H 13J 14G 14K 14M 14Q 15D 15H 15J 15N 16G 16K 17B 17J 17N 188 18M 18Q
19B 19J 19P

Base 31: 94 non-decending primes between 0 and 117:
  2   3   5   7   B   D   H   J   N   T  16  1A  1C  1G  1M  1S  1U  25  29  2B
 2H  2L  2R  34  38  3A  3E  3G  3K  47  4D  4F  4P  4R  58  5C  5I  5O  5Q  67
 6B  6D  6P  7A  7C  7G  7M  7O  89  8F  8L  8N  8T  9E  9S  AL  AR  BC  BI  BQ
 CH  CP  CT  DG  DI  DS  DU  EF  EN  ER  ET  FM  FQ  GP  GR  HK  HU  IJ  IT  JO
 JS  JU  KL  KN  KR  LM  LQ  MR  NQ  NU  OP  OT  TU 115
```
