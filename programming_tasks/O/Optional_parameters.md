[1]: http://rosettacode.org/wiki/Optional_parameters

# [Optional parameters][1]

Using named parameters:

```perl
method sorttable(:$column = 0, :$reverse, :&ordering = &infix:<cmp>) {
    my @result = self»[$column].sort: &ordering;
    return $reverse ?? @result.reverse !! @result;
}
```