[1]: http://rosettacode.org/wiki/Order_disjoint_list_items

# [Order disjoint list items][1]

```perl
sub order-disjoint-list-items(\M, \N) {
    my \bag = N.BagHash;
    M.map: { bag{$_}-- ?? N.shift !! $_ }
}
```


Testing:

```perl
for q:to/---/.comb(/ [\S+]+ % ' ' /).map({[.words]})
    the cat sat on the mat      mat cat
    the cat sat on the mat      cat mat
    A B C A B C A B C           C A C A
    A B C A B D A B E           E A D A
    A B                         B
    A B                         B A
    A B B A                     B A
    X X Y                       X
    A X                         Y A
    ---
->  $m, $n { say "\n$m ==> $n\n", order-disjoint-list-items($m, $n) }
```

#### Output:
```
the cat sat on the mat ==> mat cat
the mat sat on the cat

the cat sat on the mat ==> cat mat
the cat sat on the mat

A B C A B C A B C ==> C A C A
C B A C B A A B C

A B C A B D A B E ==> E A D A
E B C A B D A B A

A B ==> B
A B

A B ==> B A
B A

A B B A ==> B A
B A B A

X X Y ==> X
X X Y

A X ==> Y A
Y X
```