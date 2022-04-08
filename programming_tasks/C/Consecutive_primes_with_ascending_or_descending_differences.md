[1]: https://rosettacode.org/wiki/Consecutive_primes_with_ascending_or_descending_differences

# [Consecutive primes with ascending or descending differences][1]

```perl
use Math::Primesieve;
use Lingua::EN::Numbers;
 
my $sieve = Math::Primesieve.new;
 
my $limit = 1000000;
 
my @primes = $sieve.primes($limit);
 
sub runs (&op) {
    my $diff = 1;
    my $run = 1;
 
    my @diff = flat 1, (1..^@primes).map: {
        my $next = @primes[$_] - @primes[$_ - 1];
        if &op($next, $diff) { ++$run } else { $run = 1 }
        $diff = $next;
        $run;
    }
 
    my $max = max @diff;
    my @runs = @diff.grep: * == $max, :k;
 
    @runs.map( {
        my @run = (0..$max).reverse.map: -> $r { @primes[$_ - $r] }
        flat roundrobin(@run».&comma, @run.rotor(2 => -1).map({[R-] $_})».fmt('(%d)'));
    } ).join: "\n"
}
 
say "Longest run(s) of ascending prime gaps up to {comma $limit}:\n" ~ runs(&infix:«>»);
 
say "\nLongest run(s) of descending prime gaps up to {comma $limit}:\n" ~ runs(&infix:«<»);
```

#### Output:
```
Longest run(s) of ascending prime gaps up to 1,000,000:
128,981 (2) 128,983 (4) 128,987 (6) 128,993 (8) 129,001 (10) 129,011 (12) 129,023 (14) 129,037
402,581 (2) 402,583 (4) 402,587 (6) 402,593 (8) 402,601 (12) 402,613 (18) 402,631 (60) 402,691
665,111 (2) 665,113 (4) 665,117 (6) 665,123 (8) 665,131 (10) 665,141 (12) 665,153 (24) 665,177

Longest run(s) of descending prime gaps up to 1,000,000:
322,171 (22) 322,193 (20) 322,213 (16) 322,229 (8) 322,237 (6) 322,243 (4) 322,247 (2) 322,249
752,207 (44) 752,251 (12) 752,263 (10) 752,273 (8) 752,281 (6) 752,287 (4) 752,291 (2) 752,293
```
