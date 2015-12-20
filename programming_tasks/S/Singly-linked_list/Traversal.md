[1]: http://rosettacode.org/wiki/Singly-linked_list/Traversal

# [Singly-linked list/Traversal][1]

Built-in list processing in Perl is not specifically based on singly-linked lists,
but works at a higher abstraction level that encapsulates such implementation choices. Nonetheless, it's trivial to use the <tt>Pair</tt> type to build what is essentially a Lisp-style cons list, and in fact, the <tt>=&gt;</tt> pair constructor is right associative for precisely that reason.
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


It would be pretty easy to make such lists iterable as normal Perl lists,
if anyone really cared to...



Well, shoot, let's just go ahead and do it.
We'll pretend the <tt>Pair</tt> type is really a list type.
(And we show how you turn an ordinary list into a cons list using a reduction.
Note how the <tt>[=&gt;]</tt> reduction is also right associative,
just like the base operator.)

```perl
use MONKEY_TYPING;
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