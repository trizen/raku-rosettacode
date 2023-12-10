[1]: https://rosettacode.org/wiki/Base_16_numbers_needing_a_to_f

# [Base 16 numbers needing a to f][1]

Yet another poorly specced, poorly named, trivial task.



How many integers in base 16 cannot be written without using a hexadecimal digit? All of them. Or none of them.



Base 16 is not hexadecimal. Hexadecimal is *an implementation* of base 16.

```perl
use Base::Any;
set-digits <⑩ ⑪ ⑫ ⑬ ⑭ ⑮ ⑯ ⑰ ⑱ ⑲ ⑳ ㉑ ㉒ ㉓ ㉔ ㉕>;
say (7**35).&to-base(16);

# ⑭㉒⑱⑩⑰⑰⑳⑮⑱⑳⑩⑳⑱㉒㉑⑰㉒⑫⑭⑲⑯⑩㉔⑮⑰
```


How many of those glyphs are decimal digits? And yet it **is** in base 16, albeit with non-standard digit glyphs. So they **all** can be written without using a hexadecimal digit.



But wait a minute; is 2 a hexadecimal digit? Why yes, yes it is. So *none* of them can be written in hexadecimal without using a hexadecimal digit.





Bah. Show which when written in base 16, contain a digit glyph with a value greater than 9:

```perl
put display :20cols, :fmt('%3d'), (^501).grep( { so any |.map: { .polymod(16 xx *) »>» 9 } } );

sub display ($list, :$cols = 10, :$fmt = '%6d', :$title = "{+$list} matching:\n" )   {
    cache $list;
    $title ~ $list.batch($cols)».fmt($fmt).join: "\n"
}
```

#### Output:
```
301 matching:
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


But wait a minute. Let's take another look at the the task title. **Base-16 representation**. It isn't talking about Base 16 at all. It's talking about Base**-16**... so let's do it in base -16.

```perl
use Base::Any;

put display :20cols, :fmt('%3d'), (^501).grep( { .&to-base(-16).contains: /<[A..F]>/ } );

sub display ($list, :$cols = 10, :$fmt = '%6d', :$title = "{+$list} matching:\n" )   {
    cache $list;
    $title ~ $list.batch($cols)».fmt($fmt).join: "\n"
}
```

#### Output:
```
306 matching:
 10  11  12  13  14  15  16  17  18  19  20  21  22  23  24  25  26  27  28  29
 30  31  32  33  34  35  36  37  38  39  40  41  42  43  44  45  46  47  48  49
 50  51  52  53  54  55  56  57  58  59  60  61  62  63  64  65  66  67  68  69
 70  71  72  73  74  75  76  77  78  79  80  81  82  83  84  85  86  87  88  89
 90  91  92  93  94  95  96  97  98  99 100 101 102 103 104 105 106 107 108 109
110 111 122 123 124 125 126 127 138 139 140 141 142 143 154 155 156 157 158 159
170 171 172 173 174 175 186 187 188 189 190 191 202 203 204 205 206 207 218 219
220 221 222 223 234 235 236 237 238 239 250 251 252 253 254 255 266 267 268 269
270 271 272 273 274 275 276 277 278 279 280 281 282 283 284 285 286 287 288 289
290 291 292 293 294 295 296 297 298 299 300 301 302 303 304 305 306 307 308 309
310 311 312 313 314 315 316 317 318 319 320 321 322 323 324 325 326 327 328 329
330 331 332 333 334 335 336 337 338 339 340 341 342 343 344 345 346 347 348 349
350 351 352 353 354 355 356 357 358 359 360 361 362 363 364 365 366 367 378 379
380 381 382 383 394 395 396 397 398 399 410 411 412 413 414 415 426 427 428 429
430 431 442 443 444 445 446 447 458 459 460 461 462 463 474 475 476 477 478 479
490 491 492 493 494 495
```


Of course, if you are looking for the **count** of the hexadecimal numbers up to some threshold that only use "decimal" digits, it is silly and counter-productive to iterate through them and check ***each*** when you really only need to check ***one***.

```perl
use Lingua::EN::Numbers;

for 500
   ,10**8
   ,10**25
   ,10**35
   -> $threshold {
    my $limit = $threshold.base(16);
    my $i  = $limit.index: ['A'..'F'];
    quietly $limit = $limit.substr(0, $i) ~ ('9' x ($limit.chars - $i)) if $i.Str;

    for '  CAN  ', $limit,
        'CAN NOT', $threshold - $limit {
        printf( "Quantity of numbers up to %s that %s be expressed in hexadecimal without using any alphabetics: %*s\n",
         comma($threshold), $^a, comma($threshold).chars, comma($^c) )
    }

    say '';
}
```

#### Output:
```
Quantity of numbers up to 500 that   CAN   be expressed in hexadecimal without using any alphabetics: 199
Quantity of numbers up to 500 that CAN NOT be expressed in hexadecimal without using any alphabetics: 301

Quantity of numbers up to 100,000,000 that   CAN   be expressed in hexadecimal without using any alphabetics:   5,999,999
Quantity of numbers up to 100,000,000 that CAN NOT be expressed in hexadecimal without using any alphabetics:  94,000,001

Quantity of numbers up to 10,000,000,000,000,000,000,000,000 that   CAN   be expressed in hexadecimal without using any alphabetics:        845,951,614,014,849,999,999
Quantity of numbers up to 10,000,000,000,000,000,000,000,000 that CAN NOT be expressed in hexadecimal without using any alphabetics:  9,999,154,048,385,985,150,000,001

Quantity of numbers up to 100,000,000,000,000,000,000,000,000,000,000,000 that   CAN   be expressed in hexadecimal without using any alphabetics:         134,261,729,999,999,999,999,999,999,999
Quantity of numbers up to 100,000,000,000,000,000,000,000,000,000,000,000 that CAN NOT be expressed in hexadecimal without using any alphabetics:  99,999,865,738,270,000,000,000,000,000,000,001
```
