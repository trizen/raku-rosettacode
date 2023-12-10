[1]: https://rosettacode.org/wiki/Equal_prime_and_composite_sums

# [Equal prime and composite sums][1]

Let it run until I got bored and killed it. Time is total accumulated seconds since program start.

```perl
use Lingua::EN::Numbers:ver<2.8.2+>;

my $prime-sum =     [\+] (2..*).grep:  *.is-prime;
my $composite-sum = [\+] (2..*).grep: !*.is-prime;

my $c-index = 0;

for ^∞ -> $p-index {
    next if $prime-sum[$p-index] < $composite-sum[$c-index];
    printf( "%20s - %11s prime sum, %12s composite sum   %5.2f seconds\n",
      $prime-sum[$p-index].&comma, ordinal-digit($p-index + 1, :u, :c),
      ordinal-digit($c-index + 1, :u, :c), now - INIT now )
      and next if $prime-sum[$p-index] == $composite-sum[$c-index];
    ++$c-index;
    redo;
};
```

#### Output:
```
                  10 -         3ʳᵈ prime sum,          2ⁿᵈ composite sum    0.01 seconds
               1,988 -        33ʳᵈ prime sum,         51ˢᵗ composite sum    0.01 seconds
              14,697 -        80ᵗʰ prime sum,        147ᵗʰ composite sum    0.02 seconds
              83,292 -       175ᵗʰ prime sum,        361ˢᵗ composite sum    0.03 seconds
           1,503,397 -       660ᵗʰ prime sum,      1,582ⁿᵈ composite sum    0.04 seconds
          18,859,052 -     2,143ʳᵈ prime sum,      5,699ᵗʰ composite sum    0.08 seconds
          93,952,013 -     4,556ᵗʰ prime sum,     12,821ˢᵗ composite sum    0.14 seconds
      89,171,409,882 -   118,785ᵗʰ prime sum,    403,341ˢᵗ composite sum    4.23 seconds
   9,646,383,703,961 - 1,131,142ⁿᵈ prime sum,  4,229,425ᵗʰ composite sum   76.23 seconds
 209,456,854,921,713 - 5,012,372ⁿᵈ prime sum, 19,786,181ˢᵗ composite sum  968.26 seconds
^C
```
