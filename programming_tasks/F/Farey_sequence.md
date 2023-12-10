[1]: https://rosettacode.org/wiki/Farey_sequence

# [Farey sequence][1]



```perl
sub farey ($order) {
    my @l = 0/1, 1/1;
    (2..$order).map: { push @l, |(1..$^d).map: { $_/$d } }
    unique @l
}

say "Farey sequence order ";
.say for (1..11).hyper(:1batch).map: { "$_: ", .&farey.sort.map: *.nude.join('/') };
.say for (100, 200 ... 1000).race(:1batch).map: { "Farey sequence order $_ has " ~ [.&farey].elems ~ ' elements.' }
```

#### Output:
```
Farey sequence order 
1: 0/1 1/1
2: 0/1 1/2 1/1
3: 0/1 1/3 1/2 2/3 1/1
4: 0/1 1/4 1/3 1/2 2/3 3/4 1/1
5: 0/1 1/5 1/4 1/3 2/5 1/2 3/5 2/3 3/4 4/5 1/1
6: 0/1 1/6 1/5 1/4 1/3 2/5 1/2 3/5 2/3 3/4 4/5 5/6 1/1
7: 0/1 1/7 1/6 1/5 1/4 2/7 1/3 2/5 3/7 1/2 4/7 3/5 2/3 5/7 3/4 4/5 5/6 6/7 1/1
8: 0/1 1/8 1/7 1/6 1/5 1/4 2/7 1/3 3/8 2/5 3/7 1/2 4/7 3/5 5/8 2/3 5/7 3/4 4/5 5/6 6/7 7/8 1/1
9: 0/1 1/9 1/8 1/7 1/6 1/5 2/9 1/4 2/7 1/3 3/8 2/5 3/7 4/9 1/2 5/9 4/7 3/5 5/8 2/3 5/7 3/4 7/9 4/5 5/6 6/7 7/8 8/9 1/1
10: 0/1 1/10 1/9 1/8 1/7 1/6 1/5 2/9 1/4 2/7 3/10 1/3 3/8 2/5 3/7 4/9 1/2 5/9 4/7 3/5 5/8 2/3 7/10 5/7 3/4 7/9 4/5 5/6 6/7 7/8 8/9 9/10 1/1
11: 0/1 1/11 1/10 1/9 1/8 1/7 1/6 2/11 1/5 2/9 1/4 3/11 2/7 3/10 1/3 4/11 3/8 2/5 3/7 4/9 5/11 1/2 6/11 5/9 4/7 3/5 5/8 7/11 2/3 7/10 5/7 8/11 3/4 7/9 4/5 9/11 5/6 6/7 7/8 8/9 9/10 10/11 1/1
Farey sequence order 100 has 3045 elements.
Farey sequence order 200 has 12233 elements.
Farey sequence order 300 has 27399 elements.
Farey sequence order 400 has 48679 elements.
Farey sequence order 500 has 76117 elements.
Farey sequence order 600 has 109501 elements.
Farey sequence order 700 has 149019 elements.
Farey sequence order 800 has 194751 elements.
Farey sequence order 900 has 246327 elements.
Farey sequence order 1000 has 304193 elements.
```
