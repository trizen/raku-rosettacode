[1]: https://rosettacode.org/wiki/Polymorphic_copy

# [Polymorphic copy][1]



```perl
my Cool $x = 22/7 but role Fink { method brag { say "I'm a cool {self.WHAT.raku}!" }}
my Cool $y = $x.clone;
$y.brag;
```

#### Output:
```
I'm a cool Rat+{Fink}!
```
