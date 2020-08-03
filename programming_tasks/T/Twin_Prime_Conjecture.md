[1]: https://rosettacode.org/wiki/Twin_Prime_Conjecture

# [Twin Prime Conjecture][1]

```raku
use Lingua::EN::Numbers;
 
use Math::Primesieve;
 
my $p = Math::Primesieve.new;
 
printf "Twin prime pairs less than %14s: %s\n", comma(10**$_), comma $p.count(10**$_, :twins) for 1 .. 10;
```

#### Output:
```
Twin prime pairs less than             10: 2
Twin prime pairs less than            100: 8
Twin prime pairs less than          1,000: 35
Twin prime pairs less than         10,000: 205
Twin prime pairs less than        100,000: 1,224
Twin prime pairs less than      1,000,000: 8,169
Twin prime pairs less than     10,000,000: 58,980
Twin prime pairs less than    100,000,000: 440,312
Twin prime pairs less than  1,000,000,000: 3,424,506
Twin prime pairs less than 10,000,000,000: 27,412,679
```
