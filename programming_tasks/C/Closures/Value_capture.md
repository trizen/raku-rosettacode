[1]: http://rosettacode.org/wiki/Closures/Value_capture

# [Closures/Value capture][1]

All blocks are anonymous closures in Perl 6, and parameters are lexicals, so it's easy to generate a list of them. We'll use a `gather`/`take` generator loop, and call the closures in random order, just to keep things interesting.

```perl
my @c = gather for ^10 -> $i {
    take { $i * $i }
}
 
.().say for @c.pick(*);  # call them in random order
```

#### Output:
```
36
64
25
1
16
0
4
9
81
49
```


Or equivalently, using a more functional notation:

```perl
say .() for pick *, map -> $i { -> {$i * $i} }, ^10
```