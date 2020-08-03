[1]: https://rosettacode.org/wiki/Symmetric_difference

# [Symmetric difference][1]

```raku
my \A = set <John Serena Bob Mary Serena>;
my \B = set <Jim Mary John Jim Bob>;
 
say  A ∖ B; # Set subtraction
say  B ∖ A; # Set subtraction
say  A ⊖ B; # Symmetric difference
```

#### Output:
```
set(Serena)
set(Jim)
set(Jim, Serena)
```