[1]: https://rosettacode.org/wiki/Optional_parameters

# [Optional parameters][1]

Using named parameters:

```raku
method sorttable(:$column = 0, :$reverse, :&ordering = &infix:<cmp>) {
    my @result = self»[$column].sort: &ordering;
    return $reverse ?? @result.reverse !! @result;
}
```


Using optional positional parameters:

```raku
method sorttable-pos($column = 0, $reverse?, &ordering = &infix:<cmp>) {
    my @result = self»[$column].sort: &ordering;
    return $reverse ?? @result.reverse !! @result;
}
```