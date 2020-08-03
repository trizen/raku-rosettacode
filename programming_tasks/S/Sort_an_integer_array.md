[1]: https://rosettacode.org/wiki/Sort_an_integer_array

# [Sort an integer array][1]

If `@a` contains only numbers:

```raku
my @sorted = sort @a;
```


For an in-place sort:

```raku
@a .= sort;
```