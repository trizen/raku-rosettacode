[1]: http://rosettacode.org/wiki/List_comprehensions

# [List comprehensions][1]

Perl 6 has single-dimensional list comprehensions that fall out naturally from nested modifiers; multidimensional comprehensions are also supported via the cross operator; however, Perl&#160;6 does not (yet) support multi-dimensional list comprehensions with dependencies between the lists, so the most straightforward way is currently:

```perl
my $n = 20;
gather for 1..$n -> $x {
         for $x..$n -> $y {
           for $y..$n -> $z {
             take $x,$y,$z if $x*$x + $y*$y == $z*$z;
           }
         }
       }
```


Note that <tt>gather</tt>/<tt>take</tt> is the primitive in Perl&#160;6 corresponding to generators or coroutines in other languages. It is not, however, tied to function call syntax in Perl&#160;6. We can get away with that because lists are lazy, and the demand for more of the list is implicit; it does not need to be driven by function calls.