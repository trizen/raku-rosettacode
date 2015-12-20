[1]: http://rosettacode.org/wiki/Almost_prime

# [Almost prime][1]

```perl6
sub is-k-almost-prime($n is copy, $k) returns Bool {
    loop (my ($p, $f) = 2, 0; $f < $k && $p*$p <= $n; $p++) {
        $n /= $p, $f++ while $n %% $p;
    }
    $f + ($n > 1) == $k;
}
 
for 1 .. 5 -> $k {
    say .[^10]
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


Here is a solution with identical output based on the <tt>factors</tt> routine from [Count\_in\_factors#Perl\_6](/wiki/Count\_in\_factors#Perl\_6" title="Count in factors) (to be included manually until we decide where in the distribution to put it).

```perl6
constant factory = 0..* Z=> (0, 0, map { +factors($_) }, 2..*);
 
sub almost($n) { map *.key, grep *.value == $n, factory }
 
say almost($_)[^10] for 1..5;
```