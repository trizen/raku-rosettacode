[1]: https://rosettacode.org/wiki/Next_special_primes

# [Next special primes][1]

```perl
sub is-special ( ($previous, $gap) ) {
    state @primes = grep *.is-prime, 2..*;
    shift @primes while @primes[0] <= $previous + $gap;
    return ( @primes[0], @primes[0] - $previous );
}

my @specials = (2, 0), &is-special … *;

my $limit = @specials.first: :k, *.[0] > 1050;

say .fmt('%4d') for @specials.head($limit);
```

#### Output:
```
   2    0
   3    1
   5    2
  11    6
  19    8
  29   10
  41   12
  59   18
  79   20
 101   22
 127   26
 157   30
 191   34
 227   36
 269   42
 313   44
 359   46
 409   50
 461   52
 521   60
 587   66
 659   72
 733   74
 809   76
 887   78
 967   80
1049   82
```
