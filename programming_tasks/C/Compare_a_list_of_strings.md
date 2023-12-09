[1]: https://rosettacode.org/wiki/Compare_a_list_of_strings

# [Compare a list of strings][1]

In Perl 6, putting square brackets around an [infix](https://en.wikipedia.org/wiki/Infix_notation) operator turns it into a listop that effectively works as if the operator had been but in between all of the elements of the argument list *(or in technical terms, it [folds/reduces](https://en.wikipedia.org/wiki/Fold_(higher-order_function)) the list using that operator, while taking into account the operator's inherent [associativity](http://perlcabal.org/syn/S03.html#line_62) and identity value)*:

```perl
[eq] @strings  # All equal
[lt] @strings  # Strictly ascending
```