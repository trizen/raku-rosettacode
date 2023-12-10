[1]: https://rosettacode.org/wiki/Singly-linked_list/Traversal

# [Singly-linked list/Traversal][1]





### With `Pair`



Built-in list processing in Raku is not specifically based on singly-linked lists, 
but works at a higher abstraction level that encapsulates such implementation choices.  Nonetheless, it's trivial to use the `Pair` type to build what is essentially a Lisp-style cons list, and in fact, the `=>` pair constructor is right associative for precisely that reason.
We traverse such a list here using a 3-part loop:

```perl
my $list = 1 => 2 => 3 => 4 => 5 => 6 => Mu;

loop (my $l = $list; $l; $l.=value) {
    say $l.key;
}
```

#### Output:
```
1
2
3
4
5
6
```


It would be pretty easy to make such lists iterable as normal Raku lists, 
if anyone really cared to...



Well, shoot, let's just go ahead and do it.  
We'll pretend the `Pair` type is really a list type.  
(And we show how you turn an ordinary list into a cons list using a reduction.  
Note how the `[=>]` reduction is also right associative, 
just like the base operator.)

```perl
use MONKEY-TYPING;
augment class Pair {
    method traverse () {
        gather loop (my $l = self; $l; $l.=value) {
            take $l.key;
        }
    }
}

my $list = [=>] 'Ⅰ' .. 'Ⅻ', Mu;
say ~$list.traverse;
```

#### Output:
```
Ⅰ Ⅱ Ⅲ Ⅳ Ⅴ Ⅵ Ⅶ Ⅷ Ⅸ Ⅹ Ⅺ Ⅻ
```


### With custom type



Extending `class Cell` from [Singly-linked_list/Element_definition#Raku](https://rosettacode.org/wiki/Singly-linked_list/Element_definition#Raku):

```perl
    method Seq {
        self, *.next ...^ !*
    }
```


Usage:

```perl
my $list = (cons 10, (cons 20, (cons 30, Nil)));

for $list.Seq -> $cell {
    say $cell.value;
}
```

#### Output:
```
10
20
30
```
