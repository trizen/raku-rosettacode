[1]: https://rosettacode.org/wiki/Numbers_which_are_the_cube_roots_of_the_product_of_their_proper_divisors

# [Numbers which are the cube roots of the product of their proper divisors][1]

```perl
use Prime::Factor;
use Lingua::EN::Numbers;
my @cube-div = lazy 1, |(2..∞).hyper.grep: { .³ == [×] .&proper-divisors }

put "First 50 numbers which are the cube roots of the products of their proper divisors:\n" ~
  @cube-div[^50]».fmt("%3d").batch(10).join: "\n";

printf "\n%16s: %s\n", .Int.&ordinal.tc, comma @cube-div[$_ - 1] for 5e2, 5e3, 5e4;
```

#### Output:
```
First 50 numbers which are the cube roots of the products of their proper divisors:
  1  24  30  40  42  54  56  66  70  78
 88 102 104 105 110 114 128 130 135 136
138 152 154 165 170 174 182 184 186 189
190 195 222 230 231 232 238 246 248 250
255 258 266 273 282 285 286 290 296 297

  Five hundredth: 2,526

 Five thousandth: 23,118

Fifty thousandth: 223,735
```
