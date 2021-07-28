[1]: https://rosettacode.org/wiki/Strange_unique_prime_triplets

# [Strange unique prime triplets][1]



```perl
# 20210312 Raku programming solution
 
for 30, 1000 -> \k {
   given (2..k).grep(*.is-prime).combinations(3).grep(*.sum.is-prime) {
      say "Found ", +$_, " strange unique prime triplets up to ", k
   }
}
```

#### Output:
```
Found 42 strange unique prime triplets up to 30
Found 241580 strange unique prime triplets up to 1000
```
