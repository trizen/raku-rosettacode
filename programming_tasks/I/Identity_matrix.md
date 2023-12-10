[1]: https://rosettacode.org/wiki/Identity_matrix

# [Identity matrix][1]



```perl
sub identity-matrix($n) {
    my @id;
    for flat ^$n X ^$n -> $i, $j {
        @id[$i][$j] = +($i == $j);
    }
    @id;
}

.say for identity-matrix(5);
```

#### Output:
```
[1 0 0 0 0]
[0 1 0 0 0]
[0 0 1 0 0]
[0 0 0 1 0]
[0 0 0 0 1]
```


On the other hand, this may be clearer and/or faster:

```perl
sub identity-matrix($n) {
    my @id = [0 xx $n] xx $n;
    @id[$_][$_] = 1 for ^$n;
    @id;
}
```


Here is yet an other way to do it:

```perl
sub identity-matrix($n) {
    [1, |(0 xx $n-1)], *.rotate(-1) ... *[*-1]
}
```
