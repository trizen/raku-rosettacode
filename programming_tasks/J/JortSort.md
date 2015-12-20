[1]: http://rosettacode.org/wiki/JortSort

# [JortSort][1]

```perl6
sub jort-sort { @_ eqv @_.sort }
```


Actually, there's a better internal sort that seems to work best for lists that are already completely sorted, but tends to fails for any other list. The name of this sort, <tt>[!after]</tt>, is completely opaque, so we're pretty much forced to hide it inside a subroutine to prevent widespread panic.

```perl6
sub jort-sort-more-better-sorta { [!after] @_ }
```


However, since Perl 6 has a really good inliner, there's really little point anyway in using the <tt>[!after]</tt> reduction operator directly, and <tt>jort-sort-more-better-sorta</tt> is really much more self-documenting, so please don't use the reduction operator if you can. For example:


#### Output:
```
$ perl6
> [!after] <a b c>  # DON'T do it this way
True
> [!after] 1,3,2    # DON'T do it this way either
False
```


Please do your part to uphold and/or downhold our community standards.