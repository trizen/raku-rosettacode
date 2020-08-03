[1]: https://rosettacode.org/wiki/Sequence_of_primorial_primes

# [Sequence of primorial primes][1]

```raku
constant @primes     = |grep *.is-prime, 2..*;
constant @primorials = [\*] 1, @primes;
 
my @pp_indexes := |@primorials.pairs.map: {
    .key if ( .value + any(1, -1) ).is-prime
};
 
say ~ @pp_indexes[ 0 ^.. 20 ]; # Skipping bogus first element.
```

#### Output:
```
1 2 3 4 5 6 11 13 24 66 68 75 167 171 172 287 310 352 384 457
```