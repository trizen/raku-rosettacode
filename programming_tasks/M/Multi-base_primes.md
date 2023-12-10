[1]: https://rosettacode.org/wiki/Multi-base_primes

# [Multi-base primes][1]

Up to 4 character strings finish fairly quickly. 5 character strings take a while.



*All your base are belong to us. You have no chance to survive make your prime.*

```perl
use Math::Primesieve;
my $sieve = Math::Primesieve.new;

my %prime-base;

my $chars = 4; # for demonstration purposes. Change to 5 for the whole shmegegge.
 
my $threshold = ('1' ~ 'Z' x $chars).parse-base(36);

my @primes = $sieve.primes($threshold);

%prime-base.push: $_ for (2..36).map: -> $base {
    $threshold = (($base - 1).base($base) x $chars).parse-base($base);
    @primes[^(@primes.first: * > $threshold, :k)].race.map: { .base($base) => $base }
}

%prime-base.=grep: +*.value.elems > 10;

for 1 .. $chars -> $m {
    say "$m character strings that are prime in maximum bases: " ~ (my $e = ((%prime-base.grep( *.key.chars == $m )).max: +*.value.elems).value.elems);
    .say for %prime-base.grep( +*.value.elems == $e ).grep(*.key.chars == $m).sort: *.key;
    say '';
}
```

#### Output:
```
1 character strings that are prime in maximum bases: 34
2 => [3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36]

2 character strings that are prime in maximum bases: 18
21 => [3 5 6 8 9 11 14 15 18 20 21 23 26 29 30 33 35 36]

3 character strings that are prime in maximum bases: 18
131 => [4 5 7 8 9 10 12 14 15 18 19 20 23 25 27 29 30 34]
551 => [6 7 11 13 14 15 16 17 19 21 22 24 25 26 30 32 35 36]
737 => [8 9 11 12 13 15 16 17 19 22 23 24 25 26 29 30 31 36]

4 character strings that are prime in maximum bases: 19
1727 => [8 9 11 12 13 15 16 17 19 20 22 23 24 26 27 29 31 33 36]
5347 => [8 9 10 11 12 13 16 18 19 22 24 25 26 30 31 32 33 34 36]

5 character strings that are prime in maximum bases: 18
30271 => [8 10 12 13 16 17 18 20 21 23 24 25 31 32 33 34 35 36]
```


You can't really assume that the maximum string will be all numeric digits. It is just an accident that they happen to work out that way with a upper limit of base 36. If we do the same filtering using a maximum of base 62, we end up with several that contain alphabetics.

```perl
use Math::Primesieve;
use Base::Any;

my $chars = 4;
my $check-base = 62;
my $threshold = $check-base ** $chars + 20;

my $sieve = Math::Primesieve.new;
my @primes = $sieve.primes($threshold);

my %prime-base;

%prime-base.push: $_ for (2..$check-base).map: -> $base {
    $threshold = (($base - 1).&to-base($base) x $chars).&from-base($base);
    @primes[^(@primes.first: * > $threshold, :k)].race.map: { .&to-base($base) => $base }
}

%prime-base.=grep: +*.value.elems > 10;

for 1 .. $chars -> $m {
    say "$m character strings that are prime in maximum bases: " ~ (my $e = ((%prime-base.grep( *.key.chars == $m )).max: +*.value.elems).value.elems);
    .say for %prime-base.grep( +*.value.elems == $e ).grep(*.key.chars == $m).sort: *.key;
    say '';
}
```

#### Output:
```
1 character strings that are prime in maximum bases: 60
2 => [3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 51 52 53 54 55 56 57 58 59 60 61 62]

2 character strings that are prime in maximum bases: 31
65 => [7 8 9 11 13 14 16 17 18 21 22 24 27 28 29 31 32 37 38 39 41 42 43 44 46 48 51 52 57 58 59]

3 character strings that are prime in maximum bases: 33
1L1 => [22 23 25 26 27 28 29 30 31 32 33 34 36 38 39 40 41 42 43 44 45 46 48 51 52 53 54 57 58 59 60 61 62]
B9B => [13 14 15 16 17 19 20 21 23 24 26 27 28 30 31 34 36 39 40 42 45 47 49 50 52 53 54 57 58 59 60 61 62]

4 character strings that are prime in maximum bases: 32
1727 => [8 9 11 12 13 15 16 17 19 20 22 23 24 26 27 29 31 33 36 37 38 39 41 45 46 48 50 51 57 58 60 61]
417B => [12 13 15 16 17 18 19 21 23 25 28 30 32 34 35 37 38 39 41 45 48 49 50 51 52 54 56 57 58 59 61 62]
```
