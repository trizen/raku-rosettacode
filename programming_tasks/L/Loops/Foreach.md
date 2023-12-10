[1]: https://rosettacode.org/wiki/Loops/Foreach

# [Loops/Foreach][1]



```perl
say $_ for @collection;
```


Raku leaves off the `each` from `foreach`, leaving us with `for` instead. The variable `$_` refers to the current element, unless you assign a name to it using `->`.

```perl
for @collection -> $currentElement { say $currentElement; }
```


Raku will do it's best to put the topic at the right spot.

```perl
.say for @collection;
for @collection { .say };
```


Iteration can also be done with hyperoperators. In this case it's a candidate for autothreading and as such, execution order may vary. The resulting list will be in order.

```perl
@collection>>.say;
@collection>>.=&infix:<+>(2); # increment each element by 2
```
