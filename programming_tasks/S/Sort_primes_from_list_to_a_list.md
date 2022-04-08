[1]: https://rosettacode.org/wiki/Sort_primes_from_list_to_a_list

# [Sort primes from list to a list][1]

```perl
put <2 43 81 122 63 13 7 95 103>.grep( &is-prime ).sort
```

#### Output:
```
2 7 13 43 103
```


*Of course "ascending" is a little ambiguous. That ^^^ is numerically. This vvv is lexicographically.*

```perl
put <2 43 81 122 63 13 7 95 103>.grep( &is-prime ).sort: ~*
```

#### Output:
```
103 13 2 43 7
```
