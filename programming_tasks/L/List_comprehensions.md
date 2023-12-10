[1]: https://rosettacode.org/wiki/List_comprehensions

# [List comprehensions][1]


Raku has single-dimensional list comprehensions that fall out naturally from nested modifiers; multidimensional comprehensions are also supported via the cross operator; however, Raku does not (yet) support multi-dimensional list comprehensions with dependencies between the lists, so the most straightforward way is currently:

```perl
my $n = 20;
say gather for 1..$n -> $x {
         for $x..$n -> $y {
           for $y..$n -> $z {
             take $x,$y,$z if $x*$x + $y*$y == $z*$z;
           }
         }
       }
```

#### Output:
```
((3 4 5) (5 12 13) (6 8 10) (8 15 17) (9 12 15) (12 16 20))
```


Note that `gather`/`take` is the primitive in Raku corresponding to generators or coroutines in other languages.  It is not, however, tied to function call syntax in Raku.  We can get away with that because lists are lazy, and the demand for more of the list is implicit; it does not need to be driven by function calls.
