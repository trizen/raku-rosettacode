[1]: https://rosettacode.org/wiki/Primorial_numbers

# [Primorial numbers][1]

```raku
constant @primes = (1..*).grep(*.is-prime);
 
sub primorial($n) { [*] @primes[^$n] }
 
say "First ten primorials: {(primorial $_ for ^10)}";
say "primorial(10^$_) has {primorial(10**$_).chars} digits" for 1..5;
```

#### Output:
```
First ten primorials: 1 2 6 30 210 2310 30030 510510 9699690 223092870
primorial(10^1) has 10 digits
primorial(10^2) has 220 digits
primorial(10^3) has 3393 digits
primorial(10^4) has 45337 digits
^C
```


If let running long enough, it should be able to find the 100000th primorial too, but I cut it off because it was so slow. Implementing a proper prime sieve would help matters.