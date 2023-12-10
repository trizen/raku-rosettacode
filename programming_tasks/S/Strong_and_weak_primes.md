[1]: https://rosettacode.org/wiki/Strong_and_weak_primes

# [Strong and weak primes][1]



```perl
sub comma { $^i.flip.comb(3).join(',').flip }

use Math::Primesieve;

my $sieve = Math::Primesieve.new;

my @primes = $sieve.primes(10_000_019);

my (@weak, @balanced, @strong);

for 1 ..^ @primes - 1 -> $p {
    given (@primes[$p - 1] + @primes[$p + 1]) / 2 {
        when * > @primes[$p] {     @weak.push: @primes[$p] }
        when * < @primes[$p] {   @strong.push: @primes[$p] }
        default              { @balanced.push: @primes[$p] }
    }
}

for @strong,   'strong',   36,
    @weak,     'weak',     37,
    @balanced, 'balanced', 28
  -> @pr, $type, $d {
    say "\nFirst $d $type primes:\n", @pr[^$d]».&comma;
    say "Count of $type primes <=  {comma 1e6}:  ", comma +@pr[^(@pr.first: * > 1e6,:k)];
    say "Count of $type primes <= {comma 1e7}: ", comma +@pr;
}
```

#### Output:
```
First 36 strong primes:
(11 17 29 37 41 59 67 71 79 97 101 107 127 137 149 163 179 191 197 223 227 239 251 269 277 281 307 311 331 347 367 379 397 419 431 439)
Count of strong primes <=  1,000,000:  37,723
Count of strong primes <= 10,000,000: 320,991

First 37 weak primes:
(3 7 13 19 23 31 43 47 61 73 83 89 103 109 113 131 139 151 167 181 193 199 229 233 241 271 283 293 313 317 337 349 353 359 383 389 401)
Count of weak primes <=  1,000,000:  37,780
Count of weak primes <= 10,000,000: 321,750

First 28 balanced primes:
(5 53 157 173 211 257 263 373 563 593 607 653 733 947 977 1,103 1,123 1,187 1,223 1,367 1,511 1,747 1,753 1,907 2,287 2,417 2,677 2,903)
Count of balanced primes <=  1,000,000:  2,994
Count of balanced primes <= 10,000,000: 21,837
```
