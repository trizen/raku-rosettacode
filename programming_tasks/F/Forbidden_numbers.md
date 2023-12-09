[1]: https://rosettacode.org/wiki/Forbidden_numbers

# [Forbidden numbers][1]

```perl
use Lingua::EN::Numbers;
use List::Divvy;

my @f = (1..*).map: *×8-1;

my @forbidden = lazy flat @f[0], gather for ^∞ {
    state ($p0, $p1) = 1, 0;
    take (@f[$p0] < @forbidden[$p1]×4) ?? @f[$p0++] !! @forbidden[$p1++]×4;
}

put "First fifty forbidden numbers: \n" ~
  @forbidden[^50].batch(10)».fmt("%3d").join: "\n";

put "\nForbidden number count up to {.Int.&cardinal}: " ~
  comma +@forbidden.&upto: $_ for 5e2, 5e3, 5e4, 5e5, 5e6;
```

#### Output:
```
First fifty forbidden numbers: 
  7  15  23  28  31  39  47  55  60  63
 71  79  87  92  95 103 111 112 119 124
127 135 143 151 156 159 167 175 183 188
191 199 207 215 220 223 231 239 240 247
252 255 263 271 279 284 287 295 303 311

Forbidden number count up to five hundred: 82

Forbidden number count up to five thousand: 831

Forbidden number count up to fifty thousand: 8,330

Forbidden number count up to five hundred thousand: 83,331

Forbidden number count up to five million: 833,329
```
