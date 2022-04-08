[1]: https://rosettacode.org/wiki/Ultra_useful_primes

# [Ultra useful primes][1]

The first 10 take less than a quarter second. 11 through 13, a little under 30 seconds. Drops off a cliff after that.

```perl
sub useful ($n) {
    (|$n).map: {
        my $p = 1 +< ( 1 +< $_ );
        ^$p .first: ($p - *).is-prime
    }
}
 
put useful 1..10;
 
put useful 11..13;
```

#### Output:
```
1 3 5 15 5 59 159 189 569 105
1557 2549 2439
```
