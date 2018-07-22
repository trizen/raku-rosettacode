[1]: https://rosettacode.org/wiki/Abundant,_deficient_and_perfect_number_classifications

# [Abundant, deficient and perfect number classifications][1]

```perl
sub propdivsum (\x) {
    [+] flat(x > 1, gather for 2 .. x.sqrt.floor -> \d {
        my \y = x div d;
        if y * d == x { take d; take y unless y == d }
    })
}
 
say bag map { propdivsum($_) <=> $_ }, 1..20000
```

#### Output:
```
bag(Less(15043), Same(4), More(4953))
```