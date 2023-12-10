[1]: https://rosettacode.org/wiki/Singly-linked_list/Element_definition

# [Singly-linked list/Element definition][1]





#### With `Pair`



A `Pair` (constructed with the `=>` operator) can be treated as a cons cell, and thus used to build a linked lists:

```perl
my $elem = 42 => $nextelem;
```


However, because this is not the primary purpose of the `Pair` type, it suffers from the following limitations:



#### With custom type



For more flexibility, one would create a custom type:

```perl
class Cell {
    has      $.value is rw;
    has Cell $.next  is rw;
    
    # ...convenience methods here...
}

sub cons ($value, $next) { Cell.new(:$value, :$next) }

my $list = cons 10, (cons 20, (cons 30, Nil));
```
