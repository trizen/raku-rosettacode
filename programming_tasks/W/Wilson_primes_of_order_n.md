[1]: https://rosettacode.org/wiki/Wilson_primes_of_order_n

# [Wilson primes of order n][1]

```perl
# Factorial
sub postfix:<!> (Int $n) { (constant f = 1, |[\×] 1..*)[$n] }

# Invisible times
sub infix:<⁢> is tighter(&infix:<**>) { $^a * $^b };

# Prime the iterator for thread safety
sink 11000!;

my @primes = ^1.1e4 .grep: *.is-prime;

say
'  n: Wilson primes
────────────────────';

.say for (1..40).hyper(:1batch).map: -> \𝒏 { 
    sprintf "%3d: %s", 𝒏, @primes.grep( -> \𝒑 { (𝒑 ≥ 𝒏) && ((𝒏 - 1)!⁢(𝒑 - 𝒏)! - (-1) ** 𝒏) %% 𝒑² } ).Str
}
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
 12: 
 13: 13
 14: 
 15: 349
 16: 31
 17: 61 251 479
 18: 
 19: 71
 20: 59 499
 21: 
 22: 
 23: 
 24: 47 3163
 25: 
 26: 
 27: 53
 28: 347
 29: 
 30: 137 1109 5179
 31: 
 32: 71
 33: 823 1181 2927
 34: 149
 35: 71
 36: 
 37: 71 1889
 38: 
 39: 491
 40: 59 71 1171
```
