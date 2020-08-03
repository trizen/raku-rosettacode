[1]: https://rosettacode.org/wiki/Forward_difference

# [Forward difference][1]

```raku
sub dif(@array [$, *@tail]) { @tail Z- @array }
sub difn($array, $n) { ($array, &dif ... *)[$n] }
```