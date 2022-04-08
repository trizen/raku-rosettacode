[1]: https://rosettacode.org/wiki/Largest_prime_factor

# [Largest prime factor][1]

Note: These are both extreme overkill for the task requirements.



### Not particularly fast



Pure Raku. Using [Prime::Factor](https://modules.raku.org/search/?q=Prime%3A%3AFactor) from the [Raku ecosystem](https://modules.raku.org/). Makes it to 2^95 - 1 in 1 second on my system.

```perl
use Prime::Factor;
 
my $start = now;
 
for flat 600851475143, (1..∞).map: { 2 +< $_ - 1 } {
    say "Largest prime factor of $_: ", max prime-factors $_;
    last if now - $start > 1; # quit after one second of total run time
}
```

#### Output:
```
Largest prime factor of 600851475143: 6857
Largest prime factor of 3: 3
Largest prime factor of 7: 7
Largest prime factor of 15: 5
Largest prime factor of 31: 31
Largest prime factor of 63: 7
Largest prime factor of 127: 127
Largest prime factor of 255: 17
Largest prime factor of 511: 73
Largest prime factor of 1023: 31
Largest prime factor of 2047: 89
Largest prime factor of 4095: 13
Largest prime factor of 8191: 8191
Largest prime factor of 16383: 127
Largest prime factor of 32767: 151
Largest prime factor of 65535: 257
Largest prime factor of 131071: 131071
Largest prime factor of 262143: 73
Largest prime factor of 524287: 524287
Largest prime factor of 1048575: 41
Largest prime factor of 2097151: 337
Largest prime factor of 4194303: 683
Largest prime factor of 8388607: 178481
Largest prime factor of 16777215: 241
Largest prime factor of 33554431: 1801
Largest prime factor of 67108863: 8191
Largest prime factor of 134217727: 262657
Largest prime factor of 268435455: 127
Largest prime factor of 536870911: 2089
Largest prime factor of 1073741823: 331
Largest prime factor of 2147483647: 2147483647
Largest prime factor of 4294967295: 65537
Largest prime factor of 8589934591: 599479
Largest prime factor of 17179869183: 131071
Largest prime factor of 34359738367: 122921
Largest prime factor of 68719476735: 109
Largest prime factor of 137438953471: 616318177
Largest prime factor of 274877906943: 524287
Largest prime factor of 549755813887: 121369
Largest prime factor of 1099511627775: 61681
Largest prime factor of 2199023255551: 164511353
Largest prime factor of 4398046511103: 5419
Largest prime factor of 8796093022207: 2099863
Largest prime factor of 17592186044415: 2113
Largest prime factor of 35184372088831: 23311
Largest prime factor of 70368744177663: 2796203
Largest prime factor of 140737488355327: 13264529
Largest prime factor of 281474976710655: 673
Largest prime factor of 562949953421311: 4432676798593
Largest prime factor of 1125899906842623: 4051
Largest prime factor of 2251799813685247: 131071
Largest prime factor of 4503599627370495: 8191
Largest prime factor of 9007199254740991: 20394401
Largest prime factor of 18014398509481983: 262657
Largest prime factor of 36028797018963967: 201961
Largest prime factor of 72057594037927935: 15790321
Largest prime factor of 144115188075855871: 1212847
Largest prime factor of 288230376151711743: 3033169
Largest prime factor of 576460752303423487: 3203431780337
Largest prime factor of 1152921504606846975: 1321
Largest prime factor of 2305843009213693951: 2305843009213693951
Largest prime factor of 4611686018427387903: 2147483647
Largest prime factor of 9223372036854775807: 649657
Largest prime factor of 18446744073709551615: 6700417
Largest prime factor of 36893488147419103231: 145295143558111
Largest prime factor of 73786976294838206463: 599479
Largest prime factor of 147573952589676412927: 761838257287
Largest prime factor of 295147905179352825855: 131071
Largest prime factor of 590295810358705651711: 10052678938039
Largest prime factor of 1180591620717411303423: 122921
Largest prime factor of 2361183241434822606847: 212885833
Largest prime factor of 4722366482869645213695: 38737
Largest prime factor of 9444732965739290427391: 9361973132609
Largest prime factor of 18889465931478580854783: 616318177
Largest prime factor of 37778931862957161709567: 10567201
Largest prime factor of 75557863725914323419135: 525313
Largest prime factor of 151115727451828646838271: 581283643249112959
Largest prime factor of 302231454903657293676543: 22366891
Largest prime factor of 604462909807314587353087: 1113491139767
Largest prime factor of 1208925819614629174706175: 4278255361
Largest prime factor of 2417851639229258349412351: 97685839
Largest prime factor of 4835703278458516698824703: 8831418697
Largest prime factor of 9671406556917033397649407: 57912614113275649087721
Largest prime factor of 19342813113834066795298815: 14449
Largest prime factor of 38685626227668133590597631: 9520972806333758431
Largest prime factor of 77371252455336267181195263: 2932031007403
Largest prime factor of 154742504910672534362390527: 9857737155463
Largest prime factor of 309485009821345068724781055: 2931542417
Largest prime factor of 618970019642690137449562111: 618970019642690137449562111
Largest prime factor of 1237940039285380274899124223: 18837001
Largest prime factor of 2475880078570760549798248447: 23140471537
Largest prime factor of 4951760157141521099596496895: 2796203
Largest prime factor of 9903520314283042199192993791: 658812288653553079
Largest prime factor of 19807040628566084398385987583: 165768537521
Largest prime factor of 39614081257132168796771975167: 30327152671
```


### Particularly fast



Using [Perl 5](https://rosettacode.org/wiki/Category:Perl) [ntheory](https://rosettacode.org/wiki/Category:Ntheory) library via [Inline::Perl5](https://modules.raku.org/search/?q=Inline%3A%3APerl5). Makes it to about 2^155 - 1 in 1 second on my system. *Varies from 2^145-1 (lowest seen) to 2^168-1 (highest seen).*

```perl
use Inline::Perl5;
my $p5 = Inline::Perl5.new();
$p5.use: 'ntheory';
my &lpf = $p5.run('sub { ntheory::todigitstring ntheory::vecmax ntheory::factor $_[0] }');
 
my $start = now;
 
for flat 600851475143, (1..∞).map: { 2 +< $_ - 1 } {
    say "Largest prime factor of $_: ", lpf "$_";
    last if now - $start > 1; # quit after one second of total run time
}
```


Same output only much longer.
