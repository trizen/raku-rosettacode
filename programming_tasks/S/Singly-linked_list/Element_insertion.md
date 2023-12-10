[1]: https://rosettacode.org/wiki/Singly-linked_list/Element_insertion

# [Singly-linked list/Element insertion][1]





Extending `class Cell` from [Singly-linked_list/Element_definition#Raku](https://rosettacode.org/wiki/Singly-linked_list/Element_definition#Raku):

```perl
    method insert ($value) {
        $.next = Cell.new(:$value, :$.next)
    }
```
