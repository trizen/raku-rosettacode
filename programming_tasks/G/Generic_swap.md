[1]: http://rosettacode.org/wiki/Generic_swap

# [Generic swap][1]

As Perl 5. Perl 6 supports type constraints for variables and subroutines, unlike Perl 5, but the default is still to permit all values.



Alternatively, you can write it like this:

```perl
($x, $y) .= reverse;
```