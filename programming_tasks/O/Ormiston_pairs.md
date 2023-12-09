[1]: https://rosettacode.org/wiki/Ormiston_pairs

# [Ormiston pairs][1]

```perl
use Lingua::EN::Numbers;
use List::Divvy;

my @primes = lazy (^∞).hyper.grep( &is-prime ).map: { $_ => .comb.sort.join };
my @Ormistons = @primes.kv.map: { ($^value.key, @primes[$^key+1].key) if $^value.value eq @primes[$^key+1].value };

say "First thirty Ormiston pairs:"; 
say @Ormistons[^30].batch(3)».map( { "({.[0].fmt: "%5d"}, {.[1].fmt: "%5d"})" } ).join: "\n";
say '';
say +@Ormistons.&before( *[1] > $_ ) ~ " Ormiston pairs before " ~ .Int.&cardinal for 1e5, 1e6, 1e7;
```

#### Output:
```
First thirty Ormiston pairs:
( 1913,  1931) (18379, 18397) (19013, 19031)
(25013, 25031) (34613, 34631) (35617, 35671)
(35879, 35897) (36979, 36997) (37379, 37397)
(37813, 37831) (40013, 40031) (40213, 40231)
(40639, 40693) (45613, 45631) (48091, 48109)
(49279, 49297) (51613, 51631) (55313, 55331)
(56179, 56197) (56713, 56731) (58613, 58631)
(63079, 63097) (63179, 63197) (64091, 64109)
(65479, 65497) (66413, 66431) (74779, 74797)
(75913, 75931) (76213, 76231) (76579, 76597)

40 Ormiston pairs before one hundred thousand
382 Ormiston pairs before one million
3722 Ormiston pairs before ten million
```
