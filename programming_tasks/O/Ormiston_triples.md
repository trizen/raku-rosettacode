[1]: https://rosettacode.org/wiki/Ormiston_triples

# [Ormiston triples][1]

```perl
use Lingua::EN::Numbers;
use List::Divvy;

my @primes = lazy (^∞).hyper.grep( &is-prime ).map: { $_ => .comb.sort.join };
my @Ormistons = @primes.kv.map: { $^value.key if $^value.value eq @primes[$^key+1].value eq @primes[$^key+2].value};

say "First twenty-five Ormiston triples:"; 
say @Ormistons[^25].batch(5)».join(', ').join: "\n";
say '';
say +@Ormistons.&before( *[0] > $_ ) ~ " Ormiston triples before " ~ .Int.&cardinal for 1e8, 1e9;
```

#### Output:
```
First twenty-five Ormiston triples:
11117123, 12980783, 14964017, 32638213, 32964341
33539783, 35868013, 44058013, 46103237, 48015013
50324237, 52402783, 58005239, 60601237, 61395239
74699789, 76012879, 78163123, 80905879, 81966341
82324237, 82523017, 83279783, 86050781, 92514341

25 Ormiston triples before one hundred million
368 Ormiston triples before one billion
```
