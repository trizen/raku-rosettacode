[1]: https://rosettacode.org/wiki/Permuted_multiples

# [Permuted multiples][1]

```perl
put display (^∞).map(1 ~ *).race.map( -> \n { next unless [eq] (2,3,4,5,6).map: { (n × $_).comb.sort.join }; n } ).first;

sub display ($n) { join "\n", " n: $n", (2..6).map: { "×$_: {$n×$_}" } }
```

#### Output:
```
 n: 142857
×2: 285714
×3: 428571
×4: 571428
×5: 714285
×6: 857142
```
