[1]: https://rosettacode.org/wiki/Strange_plus_numbers

# [Strange plus numbers][1]

```perl
unit sub MAIN ($start = 100, $end = 500);
put +$_, " matching numbers from $start to $end:\n", $_ given
  ($start .. $end).hyper(:256batch,:8degree).grep: { all .comb.rotor(2 => -1).map: { .sum.is-prime } };
```

#### Output:
```
65 matching numbers from 100 to 500:
111 112 114 116 120 121 123 125 129 141 143 147 149 161 165 167 202 203 205 207 211 212 214 216 230 232 234 238 250 252 256 258 292 294 298 302 303 305 307 320 321 323 325 329 341 343 347 349 383 385 389 411 412 414 416 430 432 434 438 470 474 476 492 494 498
```
