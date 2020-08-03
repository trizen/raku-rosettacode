[1]: https://rosettacode.org/wiki/Return_multiple_values

# [Return multiple values][1]

Each function officially returns one value, but by returning a List or Seq you can transparently return a list of arbitrary (even infinite) size. The calling scope can destructure the list using assignment, if it so chooses:

```raku
sub addmul($a, $b) {
    $a + $b, $a * $b
}
 
my ($add, $mul) = addmul 3, 7;
```


In this example, the variable `$add` now holds the number 10, and `$mul` the number 21.