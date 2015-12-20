[1]: http://rosettacode.org/wiki/Sum_of_squares

# [Sum of squares][1]

```perl
say [+] map * ** 2, 3, 1, 4, 1, 5, 9;
```


If this expression seems puzzling, note that `* ** 2` is equivalent to `{$^x ** 2}`— the leftmost asterisk is not the multiplication operator but the `Whatever` star, which specifies currying behavior.
Another convenient way to distribute the exponentiation is via the cross metaoperator, which
as a list infix is looser than comma in precedence but tighter than the reduction list operator:

```perl
say [+] 3,1,4,1,5,9 X** 2
```