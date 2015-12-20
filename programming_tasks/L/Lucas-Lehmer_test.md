[1]: http://rosettacode.org/wiki/Lucas-Lehmer_test

# [Lucas-Lehmer test][1]

```perl
multi is_mersenne_prime(2) { True }
multi is_mersenne_prime(Int $p) {
    my $m_p = 2 ** $p - 1;
    my $s = 4;
    #  Alternate but slightly slower:   $s = ($s * $s - 2) % $m_p  for 3..$p;
    for (3 .. $p) {
      $s = $s.expmod(2, $m_p) - 2;
      $s += $m_p if $s < 0;
    }
    $s == 0;
}
 
say "M$_" if is-prime($_) and is_mersenne_prime($_) for 2..*;
```

#### Output:
```
M2
M3
M5
M7
M13
M17
M19
M31
M61
M89
M107
M127
M521
M607
M1279
M2203
M2281
M3217
M4253
M4423
^C
```