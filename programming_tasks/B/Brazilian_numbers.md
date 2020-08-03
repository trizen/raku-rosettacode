[1]: https://rosettacode.org/wiki/Brazilian_numbers

# [Brazilian numbers][1]

```perl
multi is-Brazilian (Int $n where $n %% 2 && $n > 6) { True }
 
multi is-Brazilian (Int $n) {
    LOOP: loop (my int $base = 2; $base < $n - 1; ++$base) {
        my $digit;
        for $n.polymod( $base xx * ) {
            $digit //= $_;
            next LOOP if $digit != $_;
        }
        return True
    }
    False
}
 
my $upto = 20;
 
put "First $upto Brazilian numbers:\n", (^Inf).hyper.grep( *.&is-Brazilian )[^$upto];
 
put "\nFirst $upto odd Brazilian numbers:\n", (^Inf).hyper.map( * * 2 + 1 ).grep( *.&is-Brazilian )[^$upto];
 
put "\nFirst $upto prime Brazilian numbers:\n", (^Inf).hyper(:8degree).grep( { .is-prime && .&is-Brazilian } )[^$upto];
```

#### Output:
```
First 20 Brazilian numbers:
7 8 10 12 13 14 15 16 18 20 21 22 24 26 27 28 30 31 32 33

First 20 odd Brazilian numbers:
7 13 15 21 27 31 33 35 39 43 45 51 55 57 63 65 69 73 75 77

First 20 prime Brazilian numbers:
7 13 31 43 73 127 157 211 241 307 421 463 601 757 1093 1123 1483 1723 2551 2801
```
