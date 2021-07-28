[1]: https://rosettacode.org/wiki/Erdős-primes

# [Erdős-primes][1]

```perl
use Lingua::EN::Numbers;
 
my @factorial = 1, |[\*] 1..*;
my @Erdős = ^Inf .grep: { .is-prime and none($_ «-« @factorial[^(@factorial.first: * > $_, :k)]).is-prime }
 
put 'Erdős primes < 2500:';
put @Erdős[^(@Erdős.first: * > 2500, :k)]».&comma;
put "\nThe 7,875th Erdős prime is: " ~ @Erdős[7874].&comma;
```

#### Output:
```
Erdős primes < 2500:
2 101 211 367 409 419 461 557 673 709 769 937 967 1,009 1,201 1,259 1,709 1,831 1,889 2,141 2,221 2,309 2,351 2,411 2,437

The 7,875th Erdős prime is: 999,721
```
