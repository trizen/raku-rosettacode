[1]: https://rosettacode.org/wiki/Queue/Usage

# [Queue/Usage][1]





Raku maintains the same list operators of Perl 5, for this task, the operations are:

```text
push (aka enqueue) -- @list.push
pop (aka dequeue)  -- @list.shift
empty              -- !@list.elems
```


but there's also @list.pop which removes a item from the end,
and @list.unshift which add a item on the start of the list.

Example:

```perl
my @queue = < a >;

@queue.push('b', 'c'); # [ a, b, c ]

say @queue.shift; # a
say @queue.pop; # c

say @queue; # [ b ]
say @queue.elems; # 1

@queue.unshift('A'); # [ A, b ]
@queue.push('C'); # [ A, b, C ]
```
