[1]: https://rosettacode.org/wiki/Erdős-primes

# [Erdős-primes][1]

```perl
use Lingua::EN::Numbers;

my @factorial = 1, |[\*] 1..*;
my @Erdős = ^Inf .grep: { .is-prime and none($_ «-« @factorial[^(@factorial.first: * > $_, :k)]).is-prime }

put 'Erdős primes < 2500:';
put @Erdős[^(@Erdős.first: * > 2500, :k)]».&comma;
put "\nThe 7,875th Erdős prime is: " ~ @Erdős[7874].&comma;
```

#### Output:
```
Erdős primes < 2500:
2 101 211 367 409 419 461 557 673 709 769 937 967 1,009 1,201 1,259 1,709 1,831 1,889 2,141 2,221 2,309 2,351 2,411 2,437

The 7,875th Erdős prime is: 999,721
```


Alternately, using fewer hard coded values:






```perl
use Lingua::EN::Numbers;
use List::Divvy;

my @factorial = 1, |[\*] 1..*;
my @Erdős = ^∞ .grep: { .is-prime and none($_ «-« @factorial.&upto: $_).is-prime }

put "Erdős primes < 2500:\n" ~ @Erdős.&before(2500)».&comma.batch(8)».fmt("%5s").join: "\n";
put "\nThe largest Erdős prime less than {comma 1e6.Int} is {comma .[*-1]} in {.&ordinal-digit} position."
  given @Erdős.&before(1e6);
```

#### Output:
```
Erdős primes < 2500:
    2   101   211   367   409   419   461   557
  673   709   769   937   967 1,009 1,201 1,259
1,709 1,831 1,889 2,141 2,221 2,309 2,351 2,411
2,437

The largest Erdős prime less than 1,000,000 is 999,721 in 7875th position.
```
