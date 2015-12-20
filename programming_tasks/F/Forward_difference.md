[1]: http://rosettacode.org/wiki/Forward_difference

# [Forward difference][1]

```perl6
sub dif(@array [$, *@tail]) { @tail Z- @array }
sub difn($array, $n) { ($array, &dif ... *)[$n] }
```