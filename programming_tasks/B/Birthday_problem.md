[1]: https://rosettacode.org/wiki/Birthday_problem

# [Birthday problem][1]


Gives correct answers, but more of a proof-of-concept at this point, even with max-trials at 250K it is too slow to be practical.

```perl
sub simulation ($c) {
    my $max-trials = 250_000;
    my $min-trials =   5_000;
    my $n = floor 47 * ($c-1.5)**1.5; # OEIS/A050256: 16 86 185 307
    my $N = min $max-trials, max $min-trials, 1000 * sqrt $n;

    loop {
        my $p = $N R/ elems grep { .elems > 0 }, ((grep { $_>=$c }, values bag (^365).roll($n)) xx $N);
        return($n, $p) if $p > 0.5;
        $N = min $max-trials, max $min-trials, floor 1000/(0.5-$p);
        $n++;
   }
}

printf "$_ people in a group of %s share a common birthday. (%.3f)\n", simulation($_) for 2..5;
```

#### Output:
```
2 people in a group of 23 share a common birthday. (0.506)
3 people in a group of 88 share a common birthday. (0.511)
4 people in a group of 187 share a common birthday. (0.500)
5 people in a group of 313 share a common birthday. (0.507)
```
