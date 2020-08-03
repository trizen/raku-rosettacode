[1]: https://rosettacode.org/wiki/Loop_over_multiple_arrays_simultaneously

# [Loop over multiple arrays simultaneously][1]

```raku
for <a b c> Z <A B C> Z 1, 2, 3 -> ($x, $y, $z) {
   say $x, $y, $z;
}
```


The `Z` operator stops emitting items as soon as the shortest input list is exhausted. However, short lists are easily extended by replicating all or part of the list, or by appending any kind of lazy list generator to supply default values as necessary.



Since `Z` will return a list of lists (in this example, the first list is `('a', 'A', 1)`, parentheses are used around in the lambda signature `($x, $y, $z)` to unpack the list for each iteration.



### Factoring out concatenation



Note that we can also factor out the concatenation by making the `Z` metaoperator apply the `~` concatenation operator across each triple:

```raku
.say for <a b c> Z~ <A B C> Z~ 1, 2, 3;
```


We could also use the zip-to-string with the reduction metaoperator:

```raku
.say for [Z~] [<a b c>], [<A B C>], [1,2,3]
```


### A list and its indices



The common case of iterating over a list and a list of its indices can be done using the same method:

```raku
for ^Inf Z <a b c d> -> ($i, $letter) { ... }
```


or by using the `.kv` (key and value) method on the list (and dropping the parentheses because the list returned by `.kv` is a flattened list):

```raku
for <a b c d>.kv -> $i, $letter { ... }
```