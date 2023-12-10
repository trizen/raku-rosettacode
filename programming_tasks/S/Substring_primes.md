[1]: https://rosettacode.org/wiki/Substring_primes

# [Substring primes][1]

```perl
my @p = (^10).grep: *.is-prime;

say gather while @p {
    .take for @p;

    @p = ( @p X~ <3 7> ).grep: { .is-prime and .substr(*-2,2).is-prime }
}
```

#### Output:
```
(2 3 5 7 23 37 53 73 373)
```


### Stretch Goal

```perl
my $prime-tests = 0;
my @non-primes;
sub spy-prime ($n) {
    $prime-tests++;
    my $is-p = $n.is-prime;

    push @non-primes, $n unless $is-p;
    return $is-p;
}

my @p = <2 3 5 7>;

say gather while @p {
    .take for @p;

    @p = ( @p X~ <3 7> ).grep: { !.ends-with(33|77) and .&spy-prime };
}
.say for :$prime-tests, :@non-primes;
```

#### Output:
```
(2 3 5 7 23 37 53 73 373)
prime-tests => 11
non-primes => [27 57 237 537 737 3737]
```
