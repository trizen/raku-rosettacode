[1]: http://rosettacode.org/wiki/Symmetric_difference

# [Symmetric difference][1]

```perl
my $A = set <John Serena Bob Mary Serena>;
my $B = set <Jim Mary John Jim Bob>;
 
say $A (^) $B;
```

#### Output:
```
set(Serena, Jim)
```