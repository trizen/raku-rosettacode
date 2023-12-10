[1]: https://rosettacode.org/wiki/Proper_divisors

# [Proper divisors][1]





Once your threshold is over 1000, the maximum proper divisors will always include 2, 3 and 5 as divisors, so only bother to check multiples of 2, 3 and 5.



There really isn't any point in using concurrency for a limit of 20\_000. The setup and bookkeeping drowns out any benefit. Really doesn't start to pay off until the limit is 50\_000 and higher. Try swapping in the commented out race map iterator line below for comparison.

```perl
sub propdiv (\x) {
    my @l = 1 if x > 1;
    (2 .. x.sqrt.floor).map: -> \d {
        unless x % d { @l.push: d; my \y = x div d; @l.push: y if y != d }
    }
    @l
}

put "$_ [{propdiv($_)}]" for 1..10;

my @candidates;
loop (my int $c = 30; $c <= 20_000; $c += 30) {
#(30, *+30 …^ * > 500_000).race.map: -> $c {
    my \mx = +propdiv($c);
    next if mx < @candidates - 1;
    @candidates[mx].push: $c
}

say "max = {@candidates - 1}, candidates = {@candidates.tail}";
```

#### Output:
```
1 []
2 [1]
3 [1]
4 [1 2]
5 [1]
6 [1 2 3]
7 [1]
8 [1 2 4]
9 [1 3]
10 [1 2 5]
max = 79, candidates = 15120 18480
```
