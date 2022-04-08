[1]: https://rosettacode.org/wiki/Earliest_difference_between_prime_gaps

# [Earliest difference between prime gaps][1]

1e1 through 1e7 are pretty speedy (less than 5 seconds total). 1e8 and 1e9 take several minutes.

```perl
use Math::Primesieve;
use Lingua::EN::Numbers;
 
my $iterator = Math::Primesieve::iterator.new;
my @gaps;
my $last = 2;
 
for 1..9 {
    my $m = exp $_, 10;
    my $this;
    loop {
        $this = (my $p = $iterator.next) - $last;
        if !@gaps[$this].defined {
             @gaps[$this]= $last;
             check-gap($m, $this, @gaps) && last
               if $this > 2 and @gaps[$this - 2].defined and (abs($last - @gaps[$this - 2]) > $m);
        }
        $last = $p;
    }
}
 
sub check-gap ($n, $this, @p) {
    my %upto = @p[^$this].pairs.grep: *.value;
    my @upto = (1..$this).map: { last unless %upto{$_ * 2}; %upto{$_ * 2} }
    my $key = @upto.rotor(2=>-1).first( {.sink; abs(.[0] - .[1]) > $n}, :k );
    return False unless $key;
    say "Earliest difference > {comma $n} between adjacent prime gap starting primes:";
    printf "Gap %s starts at %s, gap %s starts at %s, difference is %s\n\n",
    |(2 * $key + 2, @upto[$key], 2 * $key + 4, @upto[$key+1], abs(@upto[$key] - @upto[$key+1]))».&comma;
    True
}
```

#### Output:
```
Earliest difference > 10 between adjacent prime gap starting primes:
Gap 4 starts at 7, gap 6 starts at 23, difference is 16

Earliest difference > 100 between adjacent prime gap starting primes:
Gap 14 starts at 113, gap 16 starts at 1,831, difference is 1,718

Earliest difference > 1,000 between adjacent prime gap starting primes:
Gap 14 starts at 113, gap 16 starts at 1,831, difference is 1,718

Earliest difference > 10,000 between adjacent prime gap starting primes:
Gap 36 starts at 9,551, gap 38 starts at 30,593, difference is 21,042

Earliest difference > 100,000 between adjacent prime gap starting primes:
Gap 70 starts at 173,359, gap 72 starts at 31,397, difference is 141,962

Earliest difference > 1,000,000 between adjacent prime gap starting primes:
Gap 100 starts at 396,733, gap 102 starts at 1,444,309, difference is 1,047,576

Earliest difference > 10,000,000 between adjacent prime gap starting primes:
Gap 148 starts at 2,010,733, gap 150 starts at 13,626,257, difference is 11,615,524

Earliest difference > 100,000,000 between adjacent prime gap starting primes:
Gap 198 starts at 46,006,769, gap 200 starts at 378,043,979, difference is 332,037,210

Earliest difference > 1,000,000,000 between adjacent prime gap starting primes:
Gap 276 starts at 649,580,171, gap 278 starts at 4,260,928,601, difference is 3,611,348,430
```
