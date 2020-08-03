[1]: https://rosettacode.org/wiki/Hamming_numbers

# [Hamming numbers][1]

The limit scaling is not <em>required</em>, but it cuts down on a bunch of unnecessary calculation.

```perl
my $limit = 32;
 
sub powers_of ($radix) { 1, |[\*] $radix xx * }
 
my @hammings = 
  (   powers_of(2)[^ $limit ]       X*
      powers_of(3)[^($limit * 2/3)] X* 
      powers_of(5)[^($limit * 1/2)]
   ).sort;
 
say @hammings[^20];
say @hammings[1690]; # zero indexed
```

#### Output:
```
(1 2 3 4 5 6 8 9 10 12 15 16 18 20 24 25 27 30 32 36)
2125764000
```