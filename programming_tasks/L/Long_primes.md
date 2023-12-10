[1]: https://rosettacode.org/wiki/Long_primes

# [Long primes][1]





Not very fast as the numbers get larger.

```perl
use Math::Primesieve;
my $sieve = Math::Primesieve.new;

sub is-long (Int $p) {
    my $r = 1;
    my $rr = $r = (10 * $r) % $p for ^$p;
    my $period;
    loop {
        $r = (10 * $r) % $p;
        ++$period;
        last if $period >= $p or $r == $rr;
    }
    $period == $p - 1 and $p > 2;
}

my @primes = $sieve.primes(500);
my @long-primes = @primes.grep: {.&is-long};

put "Long primes ≤ 500:\n", @long-primes;

@long-primes = ();

for 500, 1000, 2000, 4000, 8000, 16000, 32000, 64000 -> $upto {
    state $from = 0;
    my @extend = $sieve.primes($from, $upto);
    @long-primes.append: @extend.hyper(:8degree).grep: {.&is-long};
    say "\nNumber of long primes ≤ $upto: ", +@long-primes;
    $from = $upto;
}
```

#### Output:
```
Long primes ≤ 500:
7 17 19 23 29 47 59 61 97 109 113 131 149 167 179 181 193 223 229 233 257 263 269 313 337 367 379 383 389 419 433 461 487 491 499

Number of long primes ≤ 500: 35

Number of long primes ≤ 1000: 60

Number of long primes ≤ 2000: 116

Number of long primes ≤ 4000: 218

Number of long primes ≤ 8000: 390

Number of long primes ≤ 16000: 716

Number of long primes ≤ 32000: 1300

Number of long primes ≤ 64000: 2430
```
