[1]: http://rosettacode.org/wiki/Loop_over_multiple_arrays_simultaneously

# [Loop over multiple arrays simultaneously][1]

```perl
for <a b c> Z <A B C> Z 1, 2, 3 -> $x, $y, $z {
   say $x, $y, $z;
}
```


The `Z` operator stops emitting items as soon as the shortest input list is exhausted. However, short lists are easily extended by replicating all or part of the list, or by appending any kind of lazy list generator to supply default values as necessary.



Note that we can also factor out the concatenation by making the <tt>Z</tt> metaoperator apply the <tt>~</tt> concatenation operator across each triple:

```perl
.say for <a b c> Z~ <A B C> Z~ 1, 2, 3;
```


We could also use the zip-to-string with the reduction metaoperator:

```perl
.say for [Z~] [<a b c>], [<A B C>], [1,2,3]
```
