[1]: https://rosettacode.org/wiki/Neighbour_primes

# [Neighbour primes][1]

```perl
my @primes = grep &is-prime, ^Inf;
my $last_p = @primes.first: :k, * >= 500;
my $last_q = $last_p + 1;

my @cousins = @primes.head( $last_q )
                     .rotor( 2 => -1 )
                     .map(-> (\p, \q) { p, q, p*q+2 } )
                     .grep( *.[2].is-prime );

say .fmt('%6d') for @cousins;
```

#### Output:
```
     3      5     17
     5      7     37
     7     11     79
    13     17    223
    19     23    439
    67     71   4759
   149    151  22501
   179    181  32401
   229    233  53359
   239    241  57601
   241    251  60493
   269    271  72901
   277    281  77839
   307    311  95479
   313    317  99223
   397    401 159199
   401    409 164011
   419    421 176401
   439    443 194479
   487    491 239119
```
