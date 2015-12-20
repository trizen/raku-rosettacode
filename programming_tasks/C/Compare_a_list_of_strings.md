[1]: http://rosettacode.org/wiki/Compare_a_list_of_strings

# [Compare a list of strings][1]

In Perl 6, putting square brackets around an [infix](http://en.wikipedia.org/wiki/Infix\_notation" class="extiw" title="wp:Infix notation) operator turns it into a listop that effectively works as if the operator had been but in between all of the elements of the argument list _(or in technical terms, it [folds/reduces](http://en.wikipedia.org/wiki/Fold\_(higher-order\_function)" class="extiw" title="wp:Fold (higher-order function)) the list using that operator, while taking into account the operator's inherent [associativity](http://perlcabal.org/syn/S03.html#line\_62) and identity value to Do What I Mean&#8482;)_:

```perl6
[eq] @strings  # All equal
[lt] @strings  # Strictly ascending
```