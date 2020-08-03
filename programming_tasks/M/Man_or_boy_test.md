[1]: https://rosettacode.org/wiki/Man_or_boy_test

# [Man or boy test][1]

This solution avoids creating the closure B if $k &lt;= 0 (that is, nearly every time).

```raku
sub A($k is copy, &x1, &x2, &x3, &x4, &x5) {
    $k <= 0
        ?? x4() + x5()
        !! (my &B = { A(--$k, &B, &x1, &x2, &x3, &x4) })();
};
 
say A(10, {1}, {-1}, {-1}, {1}, {0});
```

#### Output:
```
-67
```