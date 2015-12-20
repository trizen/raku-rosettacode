[1]: http://rosettacode.org/wiki/Loops/Foreach

# [Loops/Foreach][1]

```perl
say $_ for @collection;
```


Perl 6 leaves off the <tt>each</tt> from <tt>foreach</tt>, leaving us with <tt>for</tt> instead. The variable <tt>$\_</tt> refers to the current element, unless you assign a name to it using <tt>-&gt;</tt>.

```perl
for @collection -> $currentElement { say $currentElement; }
```


Perl 6 will do it's best to put the topic at the right spot.

```perl
.say for @collection;
for @collection { .say };
```


Iteration can also be done with hyperoperators. In this case it's a candidate for autothreading and as such, execution order may vary. The resulting list will be in order.

```text
@collection>>.say;
@collection>>.=&infix:<+>(2); # increment each element by 2
```