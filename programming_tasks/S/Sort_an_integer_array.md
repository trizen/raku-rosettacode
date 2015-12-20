[1]: http://rosettacode.org/wiki/Sort_an_integer_array

# [Sort an integer array][1]

If `@a` contains only numbers:

```perl
my @sorted = sort @a;
```


If some elements of `@a` are strings or are otherwise non-numeric but you want to treat them as numbers:

```perl
my @sorted = sort +*, @a;
```


For an in-place sort:

```perl
@a .= sort;
```