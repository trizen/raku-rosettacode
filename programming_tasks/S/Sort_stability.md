[1]: http://rosettacode.org/wiki/Sort_stability

# [Sort stability][1]

The [sort](http://perlcabal.org/syn/S32/Containers.html#sort) built-in (available as sub and method) is stable.



Short demonstration for sorting only on the second item of each array:

```perl6
use v6;
my @cities =
    ['UK', 'London'],
    ['US', 'New York'],
    ['US', 'Birmingham'],
    ['UK', 'Birmingham'],
    ;
 
.say for @cities.sort: { .[1] };
```