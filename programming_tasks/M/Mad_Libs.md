[1]: https://rosettacode.org/wiki/Mad_Libs

# [Mad Libs][1]

Some explanation: `S:g[...] = ...` is a global substitution that returns its result. `%` is an anonymous state variable in which we cache any results of a prompt using the `//=` operator, which assigns only if the left side is undefined. `slurp` reads an entire file from STDIN or as named in the argument list.

```perl
print S:g[ '<' (.*?) '>' ] = %.{$0} //= prompt "$0? " given slurp;
```


Sample run:


#### Output:
```
$ madlibs walk
name? Phydeaux
He or She? She
noun? flea
Phydeaux went for a walk in the park. She
found a flea. Phydeaux decided to take it home.
```