[1]: https://rosettacode.org/wiki/Abundant,_deficient_and_perfect_number_classifications

# [Abundant, deficient and perfect number classifications][1]



```perl
sub propdivsum (\x) {
    my @l = 1 if x > 1;
    (2 .. x.sqrt.floor).map: -> \d {
        unless x % d { @l.push: d; my \y = x div d; @l.push: y if y != d }
    }
    sum @l
}

say bag (1..20000).map: { propdivsum($_) <=> $_ }
```

#### Output:
```
Bag(Less(15043), More(4953), Same(4))
```
