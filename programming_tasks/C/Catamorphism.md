[1]: http://rosettacode.org/wiki/Catamorphism

# [Catamorphism][1]

Any associative infix operator, either built-in or user-defined, may be turned into a reduce operator by putting it into square brackets (known as "the reduce metaoperator") and using it as a list operator. The operations will work left-to-right or right-to-left automatically depending on the natural associativity of the base operator.

```perl6
my @list = 1..10;
say [+] @list;
say [*] @list;
say [~] @list;
say [min] @list;
say [max] @list;
say [lcm] @list;
```

#### Output:
```
55
3628800
12345678910
1
10
2520
```


In addition to the reduce metaoperator, a general higher-order function, <tt>reduce</tt>, can apply any appropriate function. Reproducing the above in this form, using the function names of those operators, we have:

```perl6
say reduce &infix:<+>, @list;
say reduce &infix:<*>, @list;
say reduce &infix:<~>, @list;
say reduce &infix:<min>, @list;
say reduce &infix:<max>, @list;
say reduce &infix:<lcm>, @list;
```