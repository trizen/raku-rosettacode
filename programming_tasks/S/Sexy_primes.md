[1]: https://rosettacode.org/wiki/Sexy_primes

# [Sexy primes][1]



```perl
use Math::Primesieve;
my $sieve = Math::Primesieve.new;

my $max = 1_000_035;
my @primes = $sieve.primes($max);

my $filter = @primes.Set;
my $primes = @primes.categorize: &sexy;

say "Total primes less than {comma $max}: ", comma +@primes;

for <pair 2 triplet 3 quadruplet 4 quintuplet 5> -> $sexy, $cnt {
    say "Number of sexy prime {$sexy}s less than {comma $max}: ", comma +$primes{$sexy};
    say "   Last 5 sexy prime {$sexy}s less than {comma $max}: ",
      join ' ', $primes{$sexy}.tail(5).grep(*.defined).map:
      { "({ $_ «+« (0,6 … 24)[^$cnt] })" }
    say '';
}

say "Number of unsexy primes less than {comma $max}: ", comma +$primes<unsexy>;
say "  Last 10 unsexy primes less than {comma $max}: ", $primes<unsexy>.tail(10);

sub sexy ($i) {
    gather {
        take 'quintuplet' if all($filter{$i «+« (6,12,18,24)});
        take 'quadruplet' if all($filter{$i «+« (6,12,18)});
        take 'triplet'    if all($filter{$i «+« (6,12)});
        take 'pair'       if $filter{$i + 6};
        take (($i >= $max - 6) && ($i + 6).is-prime) ||
          (so any($filter{$i «+« (6, -6)})) ?? 'sexy' !! 'unsexy';
    }
}

sub comma { $^i.flip.comb(3).join(',').flip }
```

#### Output:
```
Total primes less than 1,000,035: 78,500
Number of sexy prime pairs less than 1,000,035: 16,386
   Last 5 sexy prime pairs less than 1,000,035: (999371 999377) (999431 999437) (999721 999727) (999763 999769) (999953 999959)

Number of sexy prime triplets less than 1,000,035: 2,900
   Last 5 sexy prime triplets less than 1,000,035: (997427 997433 997439) (997541 997547 997553) (998071 998077 998083) (998617 998623 998629) (998737 998743 998749)

Number of sexy prime quadruplets less than 1,000,035: 325
   Last 5 sexy prime quadruplets less than 1,000,035: (977351 977357 977363 977369) (983771 983777 983783 983789) (986131 986137 986143 986149) (990371 990377 990383 990389) (997091 997097 997103 997109)

Number of sexy prime quintuplets less than 1,000,035: 1
   Last 5 sexy prime quintuplets less than 1,000,035: (5 11 17 23 29)

Number of unsexy primes less than 1,000,035: 48,627
  Last 10 unsexy primes less than 1,000,035: (999853 999863 999883 999907 999917 999931 999961 999979 999983 1000003)
```
