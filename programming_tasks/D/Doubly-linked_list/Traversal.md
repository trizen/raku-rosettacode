[1]: https://rosettacode.org/wiki/Doubly-linked_list/Traversal

# [Doubly-linked list/Traversal][1]

Since the list routines are supplied by the generic roles defined in [Doubly-linked_list/Definition#Raku](https://rosettacode.org/wiki/Doubly-linked_list/Definition#Raku), it suffices to say:

```raku
say $dll.list;
say $dll.reverse;
```


These automatically return just the payloads, hiding the elements containing the forward and backward pointers.