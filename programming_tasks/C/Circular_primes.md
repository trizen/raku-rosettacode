[1]: https://rosettacode.org/wiki/Circular_primes

# [Circular primes][1]

Most of the repunit testing is relatively speedy using the ntheory library. The really slow ones are R(25031), at ~42 seconds and R(49081) at 922(!!) seconds.

```perl
#!/usr/bin/env raku
 
# 20200406 Raku programming solution
 
sub isCircular(\n) {
   return False unless n.is-prime;
   my @circular = n.comb;
   return False if @circular.min < @circular[0];
   for 1 ..^ @circular -> $i {
      return False unless .is-prime and $_ >= n given @circular.rotate($i).join;
   }
   True
}
 
say "The first 19 circular primes are:";
say ((2..*).hyper.grep: { isCircular $_ })[^19];
 
say "\nThe next 4 circular primes, in repunit format, are:";
loop ( my $i = 7, my $count = 0; $count < 4; $i++ ) {
   ++$count, say "R($i)" if (1 x $i).is-prime
}
 
use ntheory:from<Perl5> qw[is_prime];
 
say "\nRepunit testing:";
 
(5003, 9887, 15073, 25031, 35317, 49081).map: {
    my $now = now;
    say "R($_): Prime? ", ?is_prime("{1 x $_}"), "  {(now - $now).fmt: '%.2f'}"
}
```

#### Output:
```
The first 19 circular primes are:
(2 3 5 7 11 13 17 37 79 113 197 199 337 1193 3779 11939 19937 193939 199933)

The next 4 circular primes, in repunit format, are:
R(19)
R(23)
R(317)
R(1031)

Repunit testing:
R(5003): Prime? False  0.00
R(9887): Prime? False  0.01
R(15073): Prime? False  0.02
R(25031): Prime? False  41.40
R(35317): Prime? False  0.32
R(49081): Prime? True  921.73
```
