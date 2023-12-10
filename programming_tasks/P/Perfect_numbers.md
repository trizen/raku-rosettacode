[1]: https://rosettacode.org/wiki/Perfect_numbers

# [Perfect numbers][1]


Naive (very slow) version

```perl
sub is-perf($n) { $n == [+] grep $n %% *, 1 .. $n div 2 }

# used as
put ((1..Inf).hyper.grep: {.&is-perf})[^4];
```

#### Output:
```
6 28 496 8128
```


Much, much faster version:

```perl
my @primes   = lazy (2,3,*+2 … Inf).grep: { .is-prime };
my @perfects = lazy gather for @primes {
    my $n = 2**$_ - 1;
    take $n * 2**($_ - 1) if $n.is-prime;
}

.put for @perfects[^12];
```

#### Output:
```
6
28
496
8128
33550336
8589869056
137438691328
2305843008139952128
2658455991569831744654692615953842176
191561942608236107294793378084303638130997321548169216
13164036458569648337239753460458722910223472318386943117783728128
14474011154664524427946373126085988481573677491474835889066354349131199152128
```
