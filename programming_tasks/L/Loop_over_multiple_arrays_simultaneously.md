[1]: https://rosettacode.org/wiki/Loop_over_multiple_arrays_simultaneously

# [Loop over multiple arrays simultaneously][1]





Note that all of the following work with *any* iterable object, (array, list, range, sequence; anything that does the Iterable role), not just arrays.



### Basic functionality

```perl
for <a b c> Z <A B C> Z 1, 2, 3 -> ($x, $y, $z) {
   say $x, $y, $z;
}
```


The `Z` operator stops emitting items as soon as the shortest input list is exhausted. However, short lists are easily extended by replicating all or part of the list, or by appending any kind of lazy list generator to supply default values as necessary.



Since `Z` will return a list of lists (in this example, the first list is `('a', 'A', 1)`, parentheses are used around in the lambda signature `($x, $y, $z)` to unpack the list for each iteration.



### Factoring out concatenation



Note that we can also factor out the concatenation by making the `Z` metaoperator apply the `~` concatenation operator across each triple:

```perl
.say for <a b c> Z~ <A B C> Z~ 1, 2, 3;
```


We could also use the zip-to-string with the reduction metaoperator:

```perl
.say for [Z~] <a b c>, <A B C>, (1,2,3);
```


We could also write that out "long-hand":

```perl
.say for zip :with(&infix:<~>), <a b c>, <A B C>, (1,2,3);
```


returns the exact same result so if you aren't comfortable with the concise operators, you have a choice.



### A list and its indices



The common case of iterating over a list and a list of its indices can be done using the same method:

```perl
for ^Inf Z <a b c d> -> ($i, $letter) { ... }
```


or by using the `.kv` (key and value) method on the list (and dropping the parentheses because the list returned by `.kv` is a flattened list):

```perl
for <a b c d>.kv -> $i, $letter { ... }
```


### Iterate until all exhausted



If you have different sized lists that you want to pull a value from each per iteration, but want to continue until **all** of the lists are exhausted, we have `roundrobin`.

```perl
.put for roundrobin <a b c>, 'A'..'G', ^5;
```

#### Output:
```
a A 0
b B 1
c C 2
D 3
E 4
F
G
```
