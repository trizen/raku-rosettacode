[1]: https://rosettacode.org/wiki/Almost_prime

# [Almost prime][1]

```perl
sub is-k-almost-prime($n is copy, $k) returns Bool {
    loop (my ($p, $f) = 2, 0; $f < $k && $p*$p <= $n; $p++) {
        $n /= $p, $f++ while $n %% $p;
    }
    $f + ($n > 1) == $k;
}
 
for 1 .. 5 -> $k {
    say ~.[^10]
        given grep { is-k-almost-prime($_, $k) }, 2 .. *
}
```

#### Output:
```
2 3 5 7 11 13 17 19 23 29
4 6 9 10 14 15 21 22 25 26
8 12 18 20 27 28 30 42 44 45
16 24 36 40 54 56 60 81 84 88
32 48 72 80 108 112 120 162 168 176
```


Here is a solution with identical output based on the `factors` routine from [Count_in_factors#Raku](https://rosettacode.org/wiki/Count_in_factors#Raku) (to be included manually until we decide where in the distribution to put it).

```perl
constant @primes = 2, |(3, 5, 7 ... *).grep: *.is-prime;
 
multi sub factors(1) { 1 }
multi sub factors(Int $remainder is copy) {
    gather for @primes -> $factor {
        # if remainder < factor², we're done
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
 
constant @factory = lazy 0..* Z=> flat (0, 0, map { +factors($_) }, 2..*);
 
sub almost($n) { map *.key, grep *.value == $n, @factory }
 
put almost($_)[^10] for 1..5;
```