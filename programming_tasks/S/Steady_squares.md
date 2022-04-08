[1]: https://rosettacode.org/wiki/Steady_squares

# [Steady squares][1]

```perl
.say for ({$++²}…*).kv.grep( {$^v.ends-with: $^k} )[1..10]
```

#### Output:
```
(1 1)
(5 25)
(6 36)
(25 625)
(76 5776)
(376 141376)
(625 390625)
(9376 87909376)
(90625 8212890625)
(109376 11963109376)
```
