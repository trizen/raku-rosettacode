[1]: http://rosettacode.org/wiki/Prime_decomposition

# [Prime decomposition][1]

```perl
constant @primes = 2, 3, 5, -> *@p {
    my $n = @p[*-1];
    repeat { $n += 2 } while $n %% any @p.grep: * **2 <= $n;
    $n;
} ... *;
 
sub factors(Int $remainder is copy) {
  return 1 if $remainder <= 1;
  gather for @primes -> $factor {
    if $factor * $factor > $remainder {
      take $remainder if $remainder > 1;
      last;
    }
 
    # How many times can we divide by this prime?
    while $remainder %% $factor {
        take $factor;
        last if ($remainder div= $factor) === 1;
    }
  }
}
 
say factors 536870911;
```

#### Output:
```
233 1103 2089
```