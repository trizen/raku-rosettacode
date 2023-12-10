[1]: https://rosettacode.org/wiki/Totient_function

# [Totient function][1]





This is an *incredibly* inefficient way of finding prime numbers.

```perl
use Prime::Factor;

my \𝜑 = 0, |(1..*).hyper.map: -> \t { t * [*] t.&prime-factors.squish.map: { 1 - 1/$_ } }

printf "𝜑(%2d) = %3d %s\n", $_, 𝜑[$_], $_ - 𝜑[$_] - 1 ?? '' !! 'Prime' for 1 .. 25;

(1e2, 1e3, 1e4, 1e5).map: -> $limit {
    say "\nCount of primes <= $limit: " ~ +(^$limit).grep: {$_ == 𝜑[$_] + 1}
}
```

#### Output:
```
𝜑( 1) =   1
𝜑( 2) =   1 Prime
𝜑( 3) =   2 Prime
𝜑( 4) =   2
𝜑( 5) =   4 Prime
𝜑( 6) =   2
𝜑( 7) =   6 Prime
𝜑( 8) =   4
𝜑( 9) =   6
𝜑(10) =   4
𝜑(11) =  10 Prime
𝜑(12) =   4
𝜑(13) =  12 Prime
𝜑(14) =   6
𝜑(15) =   8
𝜑(16) =   8
𝜑(17) =  16 Prime
𝜑(18) =   6
𝜑(19) =  18 Prime
𝜑(20) =   8
𝜑(21) =  12
𝜑(22) =  10
𝜑(23) =  22 Prime
𝜑(24) =   8
𝜑(25) =  20

Count of primes <= 100: 25

Count of primes <= 1000: 168

Count of primes <= 10000: 1229

Count of primes <= 100000: 9592
```
