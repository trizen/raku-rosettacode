[1]: https://rosettacode.org/wiki/Magic_constant

# [Magic constant][1]

```perl
use Lingua::EN::Numbers:ver<2.8+>;
 
my @magic-constants = lazy (3..∞).hyper.map: { (1 + .²) * $_ / 2 };
 
put "First 20 magic constants: ", @magic-constants[^20]».&comma;
say "1000th magic constant: ", @magic-constants[999].&comma;
say "\nSmallest order magic square with a constant greater than:";
 
(1..20).map: -> $p {printf "10%-2s: %s\n", $p.&super, comma 3 + @magic-constants.first( * > exp($p, 10), :k ) }
```

#### Output:
```
First 20 magic constants: 15 34 65 111 175 260 369 505 671 870 1,105 1,379 1,695 2,056 2,465 2,925 3,439 4,010 4,641 5,335
1000th magic constant: 503,006,505

Smallest order magic square with a constant greater than:
10¹ : 3
10² : 6
10³ : 13
10⁴ : 28
10⁵ : 59
10⁶ : 126
10⁷ : 272
10⁸ : 585
10⁹ : 1,260
10¹⁰: 2,715
10¹¹: 5,849
10¹²: 12,600
10¹³: 27,145
10¹⁴: 58,481
10¹⁵: 125,993
10¹⁶: 271,442
10¹⁷: 584,804
10¹⁸: 1,259,922
10¹⁹: 2,714,418
10²⁰: 5,848,036
```
