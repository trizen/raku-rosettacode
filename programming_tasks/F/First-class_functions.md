[1]: https://rosettacode.org/wiki/First-class_functions

# [First-class functions][1]


Here we use the `Z` ("zipwith") metaoperator to zip the 𝐴 and 𝐵 lists with builtin composition operator, `∘` (or just `o`).  The `.()` construct invokes the function contained in the `$_` (current topic) variable.

```perl
my \𝐴 = &sin,  &cos,  { $_ ** <3/1> }
my \𝐵 = &asin, &acos, { $_ ** <1/3> }

say .(.5) for 𝐴 Z∘ 𝐵
```

#### Output:
```
0.5
0.4999999999999999
0.5000000000000001
```


It is not clear why we don't get exactly 0.5, here.



Operators, both builtin and user-defined, are first class too.

```perl
my @a = 1,2,3;
my @op = &infix:<+>, &infix:<->, &infix:<*>;
for flat @a Z @op -> $v, &op { say 42.&op($v) }
```

#### Output:
```
43
40
126
```
