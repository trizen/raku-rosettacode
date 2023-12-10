[1]: https://rosettacode.org/wiki/Power_set

# [Power set][1]



```perl
sub powerset(Set $s) { $s.combinations.map(*.Set).Set }
say powerset set <a b c d>;
```

#### Output:
```
set(set(), set(a), set(b), set(c), set(d), set(a, b), set(a, c), set(a, d), set(b, c), set(b, d), set(c, d), set(a, b, c), set(a, b, d), set(a, c, d), set(b, c, d), set(a, b, c, d))
```


If you don't care about the actual `Set` type, the `.combinations` method by itself may be good enough for you:

```perl
.say for <a b c d>.combinations
```

#### Output:
```
 
a
b
c
d
a b
a c
a d
b c
b d
c d
a b c
a b d
a c d
b c d
a b c d
```
