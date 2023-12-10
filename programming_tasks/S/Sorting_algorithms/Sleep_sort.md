[1]: https://rosettacode.org/wiki/Sorting_algorithms/Sleep_sort

# [Sorting algorithms/Sleep sort][1]



```perl
await map -> $delay { start { sleep $delay ; say $delay } },
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


This can also be written using reactive programming:

```perl
#!/usr/bin/env raku
use v6;
react whenever Supply.from-list(@*ARGS).start({ .&sleep // +$_ }).flat { .say }
```

#### Output:
```
$ ./sleep-sort 1 3 5 6 2 4
1
2
3
4
5
6
```
