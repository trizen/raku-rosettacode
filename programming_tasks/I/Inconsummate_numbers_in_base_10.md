[1]: https://rosettacode.org/wiki/Inconsummate_numbers_in_base_10

# [Inconsummate numbers in base 10][1]

Not really pleased with this entry. It *works*, but seems inelegant.

```perl
my $upto = 1000;

my @ratios = unique (^∞).race.map({($_ / .comb.sum).narrow})[^($upto²)].grep: Int;
my @incons = (sort keys (1..$upto * 10) (-) @ratios)[^$upto];

put "First fifty inconsummate numbers (in base 10):\n" ~ @incons[^50]».fmt("%3d").batch(10).join: "\n";
put "\nOne thousandth: " ~ @incons[999]
```

#### Output:
```
First fifty inconsummate numbers (in base 10):
 62  63  65  75  84  95 161 173 195 216
261 266 272 276 326 371 372 377 381 383
386 387 395 411 416 422 426 431 432 438
441 443 461 466 471 476 482 483 486 488
491 492 493 494 497 498 516 521 522 527

One thousandth: 6996
```
