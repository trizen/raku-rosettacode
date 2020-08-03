[1]: https://rosettacode.org/wiki/Long_primes

# [Long primes][1]

Not very fast as the numbers get larger. 64000 takes a little over 15 minutes on my computer. 😕

```raku
my @long-primes = lazy (1..*).grep(*.is-prime).hyper(:8degree, :8batch).grep({1+(1/$_).base-repeating[1].chars == $_});
 
put "Long primes ≤ 500:\n", @long-primes[^(@long-primes.first: * > 500, :k)];
 
say "\nNumber of long primes ≤ $_: ", +@long-primes[^(@long-primes.first: * > $_, :k)]
  for 500, 1000, 2000, 4000, 8000, 16000, 32000, 64000;
```

#### Output:
```
Long primes ≤ 500:
7 17 19 23 29 47 59 61 97 109 113 131 149 167 179 181 193 223 229 233 257 263 269 313 337 367 379 383 389 419 433 461 487 491 499

Number of long primes ≤ 500: 35

Number of long primes ≤ 1000: 60

Number of long primes ≤ 2000: 116

Number of long primes ≤ 4000: 218

Number of long primes ≤ 8000: 390

Number of long primes ≤ 16000: 716

Number of long primes ≤ 32000: 1300

Number of long primes ≤ 64000: 2430
```