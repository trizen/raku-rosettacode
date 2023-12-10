[1]: https://rosettacode.org/wiki/Forward_difference

# [Forward difference][1]





Here we use signature matching to bind both an entire array and a version missing the head.
The Z- operator is a zip metaoperator with a minus to subtract the two lists pairwise.
It's almost a shame to define difn, since the series and subscript are hardly longer than the call itself would be, and arguably more self-documenting than the name of the function would be.

```perl
sub dif(@array [$, *@tail]) { @tail Z- @array }
sub difn($array, $n) { ($array, &dif ... *)[$n] }
```
