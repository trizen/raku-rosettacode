[1]: https://rosettacode.org/wiki/Sorting_algorithms/Sleep_sort

# [Sorting algorithms/Sleep sort][1]

```perl
await map -> $delay { start { sleep $delay ; say $delay } },
    <6 8 1 12 2 14 5 2 1 0>;
```

#### Output:
```
0
1
1
2
2
5
6
8
12
14
```