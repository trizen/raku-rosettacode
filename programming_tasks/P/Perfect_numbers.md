[1]: https://rosettacode.org/wiki/Perfect_numbers

# [Perfect numbers][1]

```raku
sub is-perf($n) { $n == [+] grep $n %% *, 1 .. $n div 2 }
 
# used as
put (grep {.&is-perf}, 1..Inf)[^4];
```

#### Output:
```
6 28 496 8128
```