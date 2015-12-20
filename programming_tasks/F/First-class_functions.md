[1]: http://rosettacode.org/wiki/First-class_functions

# [First-class functions][1]

Here we use the <tt>Z</tt> ("zipwith") metaoperator to zip the 𝐴 and 𝐵 lists with a user-defined compose function, expressed as an infix operator, <tt>∘</tt>. The <tt>.()</tt> construct invokes the function contained in the <tt>$\_</tt> (current topic) variable.

```perl6
sub infix:<∘> (&𝑔, &𝑓) { -> \x { 𝑔 𝑓 x } }
 
my \𝐴 = &sin,  &cos,  { $_ ** <3/1> }
my \𝐵 = &asin, &acos, { $_ ** <1/3> }
 
say .(.5) for 𝐴 Z∘ 𝐵
```


Output:


#### Output:
```
0.5
0.5
0.5
```


Operators, both buildin and user-defined, are first class too.

```perl6
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