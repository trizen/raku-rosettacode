[1]: http://rosettacode.org/wiki/Mad_Libs

# [Mad Libs][1]

Some explanation: <tt>S:g[...] = ...</tt> is a global substitution that returns its result. <tt>%</tt> is an anonymous state variable in which we cache any results of a prompt using the <tt>//=</tt> operator, which assigns only if the left side is undefined. <tt>slurp</tt> reads an entire file from STDIN or as named in the argument list.

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