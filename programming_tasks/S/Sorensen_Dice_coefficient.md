[1]: https://rosettacode.org/wiki/Sorensen–Dice_coefficient

# [Sorensen–Dice coefficient][1]

Using the library [Text::Sorensen](https://raku.land/github:thundergnat/Text::Sorensen) from the Raku ecosystem.



See the Raku entry for [Text completion](https://rosettacode.org/wiki/Text_completion#Sorenson-Dice) for a bespoke implementation of Sorenson-Dice. (Which is *very* similar to the library implementation.)

```perl
use Text::Sorensen :sdi;

my %tasks = './tasks.txt'.IO.slurp.lines.race.map: { $_ => .&bi-gram };

sub fuzzy-search (Str $string) { sdi($string, %tasks, :ge(.2) ).head(5).join: "\n" }

say "\n$_ >\n" ~ .&fuzzy-search for
  'Primordial primes',
  'Sunkist-Giuliani formula',
  'Sieve of Euripides',
  'Chowder numbers';
```

#### Output:
```
Primordial primes >
0.685714 Sequence of primorial primes
0.666667 Factorial primes
0.571429 Primorial numbers
0.545455 Prime words
0.521739 Almost prime

Sunkist-Giuliani formula >
0.565217 Almkvist-Giullera formula for pi
0.378378 Faulhaber's formula
0.342857 Haversine formula
0.333333 Check Machin-like formulas
0.307692 Resistance calculator

Sieve of Euripides >
0.461538 Four sides of square
0.461538 Sieve of Pritchard
0.413793 Sieve of Eratosthenes
0.4 Piprimes
0.384615 Sierpinski curve

Chowder numbers >
0.782609 Chowla numbers
0.64 Powerful numbers
0.608696 Fermat numbers
0.608696 Rhonda numbers
0.6 Lah numbers
```
