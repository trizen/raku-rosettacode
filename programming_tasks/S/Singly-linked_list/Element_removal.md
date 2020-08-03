[1]: https://rosettacode.org/wiki/Singly-linked_list/Element_removal

# [Singly-linked list/Element removal][1]

Extending `class Cell` from [Singly-linked_list/Element_definition#Raku](https://rosettacode.org/wiki/Singly-linked_list/Element_definition#Raku):

```perl
    method delete ($value --> Cell) {
        my $prev = Nil;
        my $cell = self;
        my $new-head = self;
 
        while $cell {
            my $next = $cell.next;
            if $cell.value == $value {
                $prev.next = $next if $prev;
                $cell.next = Nil;
                $new-head = $next if $cell === $new-head;
            }
            else {
                $prev = $cell;
            }
            $cell = $next;
        }
 
        return $new-head;
    }
```


Usage:

```perl
my $list = cons 10, (cons 20, (cons 10, (cons 30, Nil)));
 
$list = $list.delete(10);
```