[1]: https://rosettacode.org/wiki/Named_parameters

# [Named parameters][1]





Raku's support for optional parameters is much like Python's. Consider this declaration:

```perl
sub funkshun ($a, $b?, $c = 15, :$d, *@e, *%f) {
   ...
}
```


In the above signature:



So, if we defined the function like this:

```perl
sub funkshun ($a, $b?, :$c = 15, :$d, *@e, *%f) {
   say "$a $b $c $d";
   say join ' ', @e;
   say join ' ', keys %f;
}

# this particularly thorny call:

funkshun
    'Alfa', k1 => 'v1', c => 'Charlie', 'Bravo', 'e1',
    d => 'Delta', 'e2', k2 => 'v2';
```


would print this:


```
Alfa Bravo Charlie Delta
e1 e2
k1 k2
```
