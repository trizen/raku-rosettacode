[1]: https://rosettacode.org/wiki/Least_m_such_that_n!_%2B_m_is_prime

# [Least m such that n! + m is prime][1]

```perl
my @f = lazy flat 1, [\×] 1..*;

sink @f[700]; # pre-reify for concurrency

my @least-m = lazy (^∞).hyper(:2batch).map: {(1..*).first: -> \n {(@f[$_] + n).is-prime}};

say "Least positive m such that n! + m is prime; first fifty:\n" 
 ~ @least-m[^50].batch(10)».fmt("%3d").join: "\n";

for (1..10).map: * × 1e3 {
    my $key = @least-m.first: * > $_, :k;
    printf "\nFirst m > $_ is %d at position %d\n", @least-m[$key], $key;
}
```

#### Output:
```
Least positive m such that n! + m is prime; first fifty:
  1   1   1   1   5   7   7  11  23  17
 11   1  29  67  19  43  23  31  37  89
 29  31  31  97 131  41  59   1  67 223
107 127  79  37  97  61 131   1  43  97
 53   1  97  71  47 239 101 233  53  83

First m > 1000 is 1069 at position 107

First m > 2000 is 3391 at position 192

First m > 3000 is 3391 at position 192

First m > 4000 is 4943 at position 284

First m > 5000 is 5233 at position 384

First m > 6000 is 6131 at position 388

First m > 7000 is 9067 at position 445

First m > 8000 is 9067 at position 445

First m > 9000 is 9067 at position 445

First m > 10000 is 12619 at position 599
```
