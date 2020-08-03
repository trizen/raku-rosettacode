[1]: https://rosettacode.org/wiki/List_rooted_trees

# [List rooted trees][1]

Bags are represented by Perl 6 type [`Bag`](http://doc.perl6.org/type/Bag).

```raku
use v6;
 
multi expand-tree ( Bag $tree ) {
    bag(bag(bag()) (+) $tree) (+)
    [(+)] (
        $tree.keys ==> map {
            $^a.&expand-tree.map: * (+) ( $tree (-) bag($^a) )
        }
    );
}
 
multi expand-trees ( Bag $trees ) {
    [(+)] $trees.keys.map:  { $_.&expand-tree } ;
}      
 
my $n = 5;
for ( bag(), bag(bag()), *.&expand-trees ... * )[$n] {
    print ++$,".\t";
    .say
};
 
```

#### Output:
```
1.      bag(bag(), bag(bag()(2))) => 2
2.      bag(bag(bag()(3))) => 1
3.      bag(bag(bag(bag()), bag())) => 2
4.      bag(bag(bag(bag(bag())))) => 1
5.      bag(bag(bag())(2)) => 1
6.      bag(bag(bag(bag()(2)))) => 1
7.      bag(bag(), bag(bag(bag()))) => 2
8.      bag(bag(bag()), bag()(2)) => 2
9.      bag(bag()(4)) => 1
```


The bag `bag(bag(bag()), bag()(2))` coresponds with `((())()())`. There are two independent ways how we can get it by nesting 4 bags.