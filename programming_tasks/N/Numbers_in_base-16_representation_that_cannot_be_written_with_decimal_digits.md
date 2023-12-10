[1]: https://rosettacode.org/wiki/Numbers_in_base-16_representation_that_cannot_be_written_with_decimal_digits

# [Numbers in base-16 representation that cannot be written with decimal digits][1]

This is **literally** the exact same task as (the horribly named) [Base-16 representation](https://rosettacode.org/wiki/Base-16_representation) task.



Leaving aside the requirement that it be base 16 (well, base negative 16 according to the task title); assume it really means *hexadecimal*, otherwise all bets are off.



I challenge anyone to demonstrate how, say 465<sub>10</sub>, can be written in hexadecimal using only decimal digits.



The task as written:

```perl
put "{+$_} such numbers:\n", .batch(20)».fmt('%3d').join("\n")
    given (1..500).grep( { so any |.map: { .polymod(16 xx *) »>» 9 } } );
```

#### Output:
```
301 such numbers:
 10  11  12  13  14  15  26  27  28  29  30  31  42  43  44  45  46  47  58  59
 60  61  62  63  74  75  76  77  78  79  90  91  92  93  94  95 106 107 108 109
110 111 122 123 124 125 126 127 138 139 140 141 142 143 154 155 156 157 158 159
160 161 162 163 164 165 166 167 168 169 170 171 172 173 174 175 176 177 178 179
180 181 182 183 184 185 186 187 188 189 190 191 192 193 194 195 196 197 198 199
200 201 202 203 204 205 206 207 208 209 210 211 212 213 214 215 216 217 218 219
220 221 222 223 224 225 226 227 228 229 230 231 232 233 234 235 236 237 238 239
240 241 242 243 244 245 246 247 248 249 250 251 252 253 254 255 266 267 268 269
270 271 282 283 284 285 286 287 298 299 300 301 302 303 314 315 316 317 318 319
330 331 332 333 334 335 346 347 348 349 350 351 362 363 364 365 366 367 378 379
380 381 382 383 394 395 396 397 398 399 410 411 412 413 414 415 416 417 418 419
420 421 422 423 424 425 426 427 428 429 430 431 432 433 434 435 436 437 438 439
440 441 442 443 444 445 446 447 448 449 450 451 452 453 454 455 456 457 458 459
460 461 462 463 464 465 466 467 468 469 470 471 472 473 474 475 476 477 478 479
480 481 482 483 484 485 486 487 488 489 490 491 492 493 494 495 496 497 498 499
500
```




What the task author *probably* meant.



Find numbers in decimal that when written in hexadecimal are expressed using **only** alphabetic glyphs.



Which is a **tiny** (2 character) change from [Base-16 representation](https://rosettacode.org/wiki/Base-16_representation). Add some other (possibly useful) functionality.

```perl
#Filter out such numbers from a range:
put "Filter: {+$_} such numbers:\n", .batch(20)».fmt('%3d').join("\n")
    given (1..500).grep( { so all |.map: { .polymod(16 xx *) »>» 9 } } );

#Generate such numbers directly, up to a threshold:
put "\nGenerate: first {+$_}:\n", .batch(10)».map({ "{$_}({:16($_)})" })».fmt('%9s').join("\n") given
    ((1..^Inf).grep(* % 7).map( { .base(7).trans: [1..6] => ['A'..'F'] } )).grep(!*.contains: 0)[^42];

#Count such numbers directly, up to a threshold 
my $upto = 500;
put "\nCount: " ~ [+] flat (map {exp($_, 6)}, 1..($upto.log(16).floor)),
+(exp($upto.log(16).floor, 16) .. $upto).grep( { so all |.map: { .polymod(16 xx *) »>» 9 } });
```

#### Output:
```
Filter: 42 such numbers:
 10  11  12  13  14  15 170 171 172 173 174 175 186 187 188 189 190 191 202 203
204 205 206 207 218 219 220 221 222 223 234 235 236 237 238 239 250 251 252 253
254 255

Generate: first 42:
    A(10)     B(11)     C(12)     D(13)     E(14)     F(15)   AA(170)   AB(171)   AC(172)   AD(173)
  AE(174)   AF(175)   BA(186)   BB(187)   BC(188)   BD(189)   BE(190)   BF(191)   CA(202)   CB(203)
  CC(204)   CD(205)   CE(206)   CF(207)   DA(218)   DB(219)   DC(220)   DD(221)   DE(222)   DF(223)
  EA(234)   EB(235)   EC(236)   ED(237)   EE(238)   EF(239)   FA(250)   FB(251)   FC(252)   FD(253)
  FE(254)   FF(255)

Count: 42
```
