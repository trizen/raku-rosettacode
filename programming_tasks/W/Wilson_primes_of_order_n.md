[1]: https://rosettacode.org/wiki/Wilson_primes_of_order_n

# [Wilson primes of order n][1]

```perl
sub postfix:<!> (Int $n) { (constant f = 1, |[\×] 1..*)[$n] }
 
my @primes = ^1.1e4 .grep: *.is-prime;
 
say
'  n: Wilson primes
────────────────────';
 
-> \n { printf "%3d: %s\n", n, @primes.grep( ->\p { (p ≥ n) && ((n - 1)! × (p - n)! - (-1) ** n) %% p² } ).Str } for 1..11;
```

#### Output:
```
  n: Wilson primes
────────────────────
  1: 5 13 563
  2: 2 3 11 107 4931
  3: 7
  4: 10429
  5: 5 7 47
  6: 11
  7: 17
  8: 
  9: 541
 10: 11 1109
 11: 17 2713
```
