[1]: https://rosettacode.org/wiki/Primorial_numbers

# [Primorial numbers][1]





### Native Raku



With the module `Math::Primesieve`, this runs in about 1/3 the time vs the built in prime generator.

```perl
use Math::Primesieve;

my $sieve = Math::Primesieve.new;
my @primes = $sieve.primes(10_000_000);

sub primorial($n) { [*] @primes[^$n] }

say "First ten primorials: {(primorial $_ for ^10)}";
say "primorial(10^$_) has {primorial(10**$_).chars} digits" for 1..5;
```

#### Output:
```
First ten primorials: 1 2 6 30 210 2310 30030 510510 9699690 223092870
primorial(10^1) has 10 digits
primorial(10^2) has 220 digits
primorial(10^3) has 3393 digits
primorial(10^4) has 45337 digits
primorial(10^5) has 563921 digits
```


### Imported library



For a real speed boost, load the Perl 5 ntheory library.

```perl
use Lingua::EN::Numbers;
use ntheory:from<Perl5> <pn_primorial>;

say "First ten primorials: ", ^10 .map( { pn_primorial($_) } ).join: ', ';

 for 1..8 {
    my $now = now;
    printf "primorial(10^%d) has %-11s digits - %s\n", $_,
        comma(pn_primorial(10**$_).Str.chars),
        "Elapsed seconds: {(now - $now).round: .001}";
}
```

#### Output:
```
First ten primorials: 1, 2, 6, 30, 210, 2310, 30030, 510510, 9699690, 223092870
primorial(10^1) has 10          digits - Elapsed seconds: 0.002
primorial(10^2) has 220         digits - Elapsed seconds: 0.015
primorial(10^3) has 3,393       digits - Elapsed seconds: 0.001
primorial(10^4) has 45,337      digits - Elapsed seconds: 0.005
primorial(10^5) has 563,921     digits - Elapsed seconds: 0.139
primorial(10^6) has 6,722,809   digits - Elapsed seconds: 3.015
primorial(10^7) has 77,919,922  digits - Elapsed seconds: 56.483
primorial(10^8) has 885,105,237 digits - Elapsed seconds: 981.71
```
