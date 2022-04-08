[1]: https://rosettacode.org/wiki/Find_square_difference

# [Find square difference][1]

```perl
say first { $_² - ($_-1)² > 1000 }, ^Inf;
```

#### Output:
```
501
```
