[1]: https://rosettacode.org/wiki/Catalan_numbers/Pascal's_triangle

# [Catalan numbers/Pascal's triangle][1]

```perl
constant @pascal = [1], -> @p { [0, |@p Z+ |@p, 0] } ... *;
 
constant @catalan = gather for 2, 4 ... * -> $ix {
    my @row := @pascal[$ix];
    my $mid = +@row div 2;
    take [-] @row[$mid, $mid+1]
}
 
.say for @catalan[^20];
```

#### Output:
```
1
2
5
14
42
132
429
1430
4862
16796
58786
208012
742900
2674440
9694845
35357670
129644790
477638700
1767263190
6564120420
```