[1]: http://rosettacode.org/wiki/Factors_of_a_Mersenne_number

# [Factors of a Mersenne number][1]

```perl
my @primes = 2, 3, -> $n is copy {
    repeat { $n += 2 } until $n %% none do for @primes -> $p {
        last if $p > sqrt($n);
        $p;
    }
    $n;
} ... *;
 
multi factors(1) { 1 }
multi factors(Int $remainder is copy) {
  gather for @primes -> $factor {
    if $factor * $factor > $remainder {
      take $remainder if $remainder > 1;
      last;
    }
    while $remainder %% $factor {
        take $factor;
        last if ($remainder div= $factor) === 1;
    }
  }
}
 
sub is_prime($x) { (state %){$x} //= factors($x) == 1 }
 
sub mtest($bits, $p) {
    my @bits = $bits.base(2).comb;
    loop (my $sq = 1; @bits; $sq %= $p) {
	$sq *= $sq;
	$sq += $sq if 1 == @bits.shift;
    }
    $sq == 1;
}
 
for flat 2 .. 60, 929 -> $m {
    next unless is_prime($m);
    my $f = 0;
    my $x = 2**$m - 1;
    my $q;
    for 1..* -> $k {
	$q = 2 * $k * $m + 1;
	next unless $q % 8 == 1|7 or is_prime($q);
	last if $q * $q > $x or $f = mtest($m, $q);
    }
 
    say $f ?? "M$m = $x\n\t= $q × { $x div $q }"
           !! "M$m = $x is prime";
}
```

#### Output:
```
M2 = 3 is prime
M3 = 7 is prime
M5 = 31 is prime
M7 = 127 is prime
M11 = 2047
        = 23 × 89
M13 = 8191 is prime
M17 = 131071 is prime
M19 = 524287 is prime
M23 = 8388607
        = 47 × 178481
M29 = 536870911
        = 233 × 2304167
M31 = 2147483647 is prime
M37 = 137438953471
        = 223 × 616318177
M41 = 2199023255551
        = 13367 × 164511353
M43 = 8796093022207
        = 431 × 20408568497
M47 = 140737488355327
        = 2351 × 59862819377
M53 = 9007199254740991
        = 6361 × 1416003655831
M59 = 576460752303423487
        = 179951 × 3203431780337
M929 = 4538015467766671944574165338592225830478699345884382504442663144885072806275648112625635725391102144133907238129251016389326737199538896813326509341743147661691195191795226666084858428449394948944821764472508048114220424520501343042471615418544488778723282182172070046459244838911
        = 13007 × 348890248924938259750454781163390930305120269538278042934009621348894657205785201247454118966026150852149399410259938217062100192168747352450719561908445272675574320888385228421992652298715687625495638077382028762529439880103124705348782610789919949159935587158612289264184273
```