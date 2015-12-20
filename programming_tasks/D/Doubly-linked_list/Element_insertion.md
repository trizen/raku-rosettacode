[1]: http://rosettacode.org/wiki/Doubly-linked_list/Element_insertion

# [Doubly-linked list/Element insertion][1]

Using the generic definitions from [Doubly-linked_list/Definition#Perl_6](http://rosettacode.org/wiki/Doubly-linked_list/Definition#Perl_6):

```perl
class DLElem_Str does DLElem[Str] {}
class DLList_Str does DLList[DLElem_Str] {}
 
my $sdll = DLList_Str.new;
my $b = $sdll.first.post-insert('A').post-insert('B');
$b.pre-insert('C');
say $sdll.list;  # A C B
```

#### Output:
```
A C B
```