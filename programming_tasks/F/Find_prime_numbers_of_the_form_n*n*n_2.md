[1]: https://rosettacode.org/wiki/Find_prime_numbers_of_the_form_n*n*n%2B2

# [Find prime numbers of the form n*n*n+2][1]

```perl
# 20210315 Raku programming solution
 
say ((1…199)»³ »+»2).grep: *.is-prime
```

#### Output:
```
(3 29 127 24391 91127 250049 274627 328511 357913 571789 1157627 1442899 1860869 2146691 2924209 3581579 5000213 5177719 6751271)
```
