[1]: https://rosettacode.org/wiki/Safe_primes_and_unsafe_primes

# [Safe primes and unsafe primes][1]

Perl&#160;6 has a built-in method .is-prime to test for prime numbers. It's great for testing individual numbers or to find/filter a few thousand numbers, but when you are looking for millions, it becomes a drag. No fear, the Perl&#160;6 ecosystem has a fast prime sieve module available which can produce 10 million primes in a few seconds. Once we have the primes, it is just a small matter of filtering and formatting them appropriately.

```perl
sub comma { $^i.flip.comb(3).join(',').flip }
 
use Math::Primesieve;
 
my $sieve = Math::Primesieve.new;
 
my %primes = $sieve.primes(10_000_000) X=> 1;
 
my $primes = %primes.keys.classify: { %primes{($_ - 1)/2} ?? 'safe' !! 'unsafe' };
 
for 'safe', 35, 'unsafe', 40 -> $type, $quantity {
    say "The first $quantity $type primes are:";
 
    say $primes{$type}.sort(+*)[^$quantity]».&comma;
 
    say "The number of $type primes up to {comma $_}: ", comma +$primes{$type}.hyper.grep: * < $_ for 1e6, 1e7;
 
    say '';
}
```

#### Output:
```
The first 35 safe primes are:
(5 7 11 23 47 59 83 107 167 179 227 263 347 359 383 467 479 503 563 587 719 839 863 887 983 1,019 1,187 1,283 1,307 1,319 1,367 1,439 1,487 1,523 1,619)
The number of safe primes up to 1,000,000: 4,324
The number of safe primes up to 10,000,000: 30,657

The first 40 unsafe primes are:
(2 3 13 17 19 29 31 37 41 43 53 61 67 71 73 79 89 97 101 103 109 113 127 131 137 139 149 151 157 163 173 181 191 193 197 199 211 223 229 233)
The number of unsafe primes up to 1,000,000: 74,174
The number of unsafe primes up to 10,000,000: 633,922
```