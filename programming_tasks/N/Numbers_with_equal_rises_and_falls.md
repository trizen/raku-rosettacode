[1]: https://rosettacode.org/wiki/Numbers_with_equal_rises_and_falls

# [Numbers with equal rises and falls][1]

```perl
use Lingua::EN::Numbers;
use Base::Any;

sub rf (int $base = 10, $batch = Any, &op = &infix:<==>) {
    my %batch = batch => $batch if $batch;
    flat (1 .. ∞).hyper(|%batch).map: {
        my int ($this, $last) = $_, $_ % $base;
        my int ($rise, $fall) = 0, 0;
        while $this {
            my int $rem = $this % $base;
            $this = $this div $base;
            if    $rem > $last { $fall = $fall + 1 }
            elsif $rem < $last { $rise = $rise + 1 }
            $last = $rem
        }
        next unless &op($rise, $fall);
        $_
    }
}

# The task
my $upto = 200;
put "Rise = Fall:\nFirst {$upto.&cardinal} (base 10):";
.put for rf[^$upto]».fmt("%3d").batch(20);

$upto = 10_000_000;
put "\nThe {$upto.&ordinal} (base 10): ", comma rf(10, 65536)[$upto - 1];

# Other bases and comparisons
put "\n\nGeneralized for other bases and other comparisons:";
$upto = ^5;
my $which = "{tc $upto.map({.exp(10).&ordinal}).join: ', '}, values in some other bases:";

put "\nRise = Fall: $which";
for <3 296691 4 296694 5 296697 6 296700 7 296703 8 296706 9 296709 10 296712
     11 296744 12 296747 13 296750 14 296753 15 296756 16 296759 20 296762 60 296765>
  -> $base, $oeis {
    put "Base {$base.fmt(<%2d>)} (https://oeis.org/A$oeis): ",
    $upto.map({rf(+$base, Any)[.exp(10) - 1].&to-base($base)}).join: ', '
}

put "\nRise > Fall: $which";
for <3 296692 4 296695 5 296698 6 296701 7 296704 8 296707 9 296710 10 296713
     11 296745 12 296748 13 296751 14 296754 15 296757 16 296760 20 296763 60 296766>
  -> $base, $oeis {
     put "Base {$base.fmt(<%2d>)} (https://oeis.org/A$oeis): ",
     $upto.map({rf(+$base, Any, &infix:«>»)[.exp(10) - 1].&to-base($base)}).join: ', '
 }

put "\nRise < Fall: $which";
for <3 296693 4 296696 5 296699 6 296702 7 296705 8 296708 9 296711 10 296714
     11 296746 12 296749 13 296752 14 296755 15 296758 16 296761 20 296764 60 296767>
  -> $base, $oeis {
     put "Base {$base.fmt(<%2d>)} (https://oeis.org/A$oeis): ",
     $upto.map({rf(+$base, Any, &infix:«<»)[.exp(10) - 1].&to-base($base)}).join: ', '
 }
```

#### Output:
```
Rise = Fall:
First two hundred (base 10):
  1   2   3   4   5   6   7   8   9  11  22  33  44  55  66  77  88  99 101 102
103 104 105 106 107 108 109 111 120 121 130 131 132 140 141 142 143 150 151 152
153 154 160 161 162 163 164 165 170 171 172 173 174 175 176 180 181 182 183 184
185 186 187 190 191 192 193 194 195 196 197 198 201 202 203 204 205 206 207 208
209 212 213 214 215 216 217 218 219 222 230 231 232 240 241 242 243 250 251 252
253 254 260 261 262 263 264 265 270 271 272 273 274 275 276 280 281 282 283 284
285 286 287 290 291 292 293 294 295 296 297 298 301 302 303 304 305 306 307 308
309 312 313 314 315 316 317 318 319 323 324 325 326 327 328 329 333 340 341 342
343 350 351 352 353 354 360 361 362 363 364 365 370 371 372 373 374 375 376 380
381 382 383 384 385 386 387 390 391 392 393 394 395 396 397 398 401 402 403 404

The ten millionth (base 10): 41,909,002


Generalized for other bases and other comparisons:

Rise = Fall: First, tenth, one hundredth, one thousandth, ten thousandth, values in some other bases:
Base  3 (https://oeis.org/A296691): 1, 201, 22112, 10101111, 1100022001
Base  4 (https://oeis.org/A296694): 1, 111, 3333, 221012, 13002120
Base  5 (https://oeis.org/A296697): 1, 102, 1441, 40011, 1431201
Base  6 (https://oeis.org/A296700): 1, 55, 512, 20424, 400402
Base  7 (https://oeis.org/A296703): 1, 44, 365, 12620, 155554
Base  8 (https://oeis.org/A296706): 1, 33, 316, 7466, 60404
Base  9 (https://oeis.org/A296709): 1, 22, 275, 5113, 40217
Base 10 (https://oeis.org/A296712): 1, 11, 252, 3396, 29201
Base 11 (https://oeis.org/A296744): 1, A, 216, 2240, 21718
Base 12 (https://oeis.org/A296747): 1, A, 201, 10AA, 19723
Base 13 (https://oeis.org/A296750): 1, A, 1B8, A0A, 172A7
Base 14 (https://oeis.org/A296753): 1, A, 1B5, 8B9, 14B81
Base 15 (https://oeis.org/A296756): 1, A, 1B2, 7D4, 11BBA
Base 16 (https://oeis.org/A296759): 1, A, 1A9, 716, 10424
Base 20 (https://oeis.org/A296762): 1, A, 196, 523, 8011
Base 60 (https://oeis.org/A296765): 1, A, ff, 1f2, 63Q

Rise > Fall: First, tenth, one hundredth, one thousandth, ten thousandth, values in some other bases:
Base  3 (https://oeis.org/A296692): 12, 1222, 122202, 12222001, 2001200001
Base  4 (https://oeis.org/A296695): 12, 233, 12113, 1003012, 13131333
Base  5 (https://oeis.org/A296698): 12, 122, 2302, 112013, 1342223
Base  6 (https://oeis.org/A296701): 12, 45, 1305, 20233, 333134
Base  7 (https://oeis.org/A296704): 12, 34, 1166, 11612, 140045
Base  8 (https://oeis.org/A296707): 12, 26, 1013, 4557, 106756
Base  9 (https://oeis.org/A296710): 12, 25, 348, 2808, 36781
Base 10 (https://oeis.org/A296713): 12, 24, 249, 2345, 23678
Base 11 (https://oeis.org/A296745): 12, 23, 223, 1836, 15806
Base 12 (https://oeis.org/A296748): 12, 1B, 166, 1623, 12534
Base 13 (https://oeis.org/A296751): 12, 1B, 145, 149B, A069
Base 14 (https://oeis.org/A296754): 12, 1B, 12B, 1393, 6BC9
Base 15 (https://oeis.org/A296757): 12, 1B, 11A, 12B7, 568E
Base 16 (https://oeis.org/A296760): 12, 1B, CD, 1206, 466A
Base 20 (https://oeis.org/A296763): 12, 1B, 7E, 6BF, 2857
Base 60 (https://oeis.org/A296766): 12, 1B, 2i, Lp, 66U

Rise < Fall: First, tenth, one hundredth, one thousandth, ten thousandth, values in some other bases:
Base  3 (https://oeis.org/A296693): 10, 221, 22220, 10021001, 1012110000
Base  4 (https://oeis.org/A296696): 10, 210, 3330, 231210, 13132000
Base  5 (https://oeis.org/A296699): 10, 43, 2420, 43033, 2030042
Base  6 (https://oeis.org/A296702): 10, 43, 1540, 25543, 403531
Base  7 (https://oeis.org/A296705): 10, 43, 1010, 10051, 206260
Base  8 (https://oeis.org/A296708): 10, 43, 660, 5732, 75051
Base  9 (https://oeis.org/A296711): 10, 43, 643, 5010, 60873
Base 10 (https://oeis.org/A296714): 10, 43, 621, 4120, 44100
Base 11 (https://oeis.org/A296746): 10, 43, 544, 3243, 31160
Base 12 (https://oeis.org/A296749): 10, 43, 520, 2A71, 18321
Base 13 (https://oeis.org/A296752): 10, 43, 422, 2164, B624
Base 14 (https://oeis.org/A296755): 10, 43, 310, 1CA3, A506
Base 15 (https://oeis.org/A296758): 10, 43, E8, 1A20, 9518
Base 16 (https://oeis.org/A296761): 10, 43, E8, 10D0, 860D
Base 20 (https://oeis.org/A296764): 10, 43, E8, G33, 5F43
Base 60 (https://oeis.org/A296767): 10, 43, E8, j9, ZUT
```
