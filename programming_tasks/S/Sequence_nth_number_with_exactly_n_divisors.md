[1]: https://rosettacode.org/wiki/Sequence:_nth_number_with_exactly_n_divisors

# [Sequence: nth number with exactly n divisors][1]

[Try it online!](https://tio.run/##dVLbTsJAEH3nKw4IpgW6ATQY2QAao4kvmsijEFPoVpr05nZr2hh@yk/wx@p0CwUf3JfuzJwzc/ZMYyH9cVEk6RqO92ltojRUMJaZia8G6EihUhliBM9FxrzEiqUXCK5rPde3CTwEY1RLPqRirh9F0mSBHU9gzbB09m3Kk4a@SBJk6IDSCHIsc0wppsFwOCiYUmU@p1uzCSPvwzGx0/xdg74NorR9L/AU0UYDXmVutKKEUu9SxNS4VoldH0PGuryxx7xW7BXHGSqE2grYUto5w@Lu6YU6xqlC68GTiTrMUkIGCSIXz/ePi8nt4OriejhucY00qH8FM9k2j4U0JqO1rTbbowftcO@BQbcZLmGHDiVrlSa9WNdFFpcQC8M@asE6XplkiMY40YmhpR0evXvA/6ZIsK0iSRWidzq0PPK0VNo1tbH6Vunr/nwfyTWTueX7JyejyhOKTB2WSI1pWey8/mf4v9BerxRZavmLab/VYb1jXhS/)

```perl
sub div-count (\x) {
    return 2 if x.is-prime;
    +flat (1 .. x.sqrt.floor).map: -> \d {
        unless x % d { my \y = x div d; y == d ?? y !! (y, d) }
    }
}
 
my $limit = 20;
 
my @primes = grep { .is-prime }, 1..*;
@primes[$limit]; # prime the array. SCNR
 
put "First $limit terms of OEIS:A073916";
put (1..$limit).hyper(:2batch).map: -> $n {
    ($n > 4 and $n.is-prime) ??
    exp($n - 1, @primes[$n - 1]) !!
    do {
        my $i = 0;
        my $iterator = $n %% 2 ?? (1..*) !! (1..*).map: *²;
        $iterator.first: {
            next unless $n == .&div-count;
            next unless ++$i == $n;
            $_
        }
    }
};
```

#### Output:
```
First 20 terms of OEIS:A073916
1 3 25 14 14641 44 24137569 70 1089 405 819628286980801 160 22563490300366186081 2752 9801 462 21559177407076402401757871041 1044 740195513856780056217081017732809 1520
```