[1]: http://rosettacode.org/wiki/Currying

# [Currying][1]

All callable objects have an "assuming" method that can do partial application of either positional or named arguments. Here we curry the built-in subtraction operator.

```perl
my &negative = &infix:<->.assuming(0);
say negative 1;
```

#### Output:
```
-1
```