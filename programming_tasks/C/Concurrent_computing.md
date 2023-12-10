[1]: https://rosettacode.org/wiki/Concurrent_computing

# [Concurrent computing][1]



```perl
my @words = <Enjoy Rosetta Code>;
@words.race(:batch(1)).map: { sleep rand; say $_ };
```

#### Output:
```
Code
Rosetta
Enjoy
```
