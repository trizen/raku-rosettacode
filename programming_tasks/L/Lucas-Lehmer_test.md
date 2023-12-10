[1]: https://rosettacode.org/wiki/Lucas-Lehmer_test

# [Lucas-Lehmer test][1]



```perl
multi is_mersenne_prime(2) { True }
multi is_mersenne_prime(Int $p) {
    my $m_p = 2 ** $p - 1;
    my $s = 4;
    $s = $s.expmod(2, $m_p) - 2 for 3 .. $p;
    !$s
}

.say for (2,3,5,7 … *).hyper(:8degree).grep( *.is-prime ).map: { next unless .&is_mersenne_prime; "M$_" };
```


Letting it run for about a minute...


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
M9689
M9941
M11213
^C

real    0m55.527s
user    6m47.106s
sys     0m0.404s
```
