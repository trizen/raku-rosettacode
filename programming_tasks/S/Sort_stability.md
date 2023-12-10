[1]: https://rosettacode.org/wiki/Sort_stability

# [Sort stability][1]





The [sort](https://docs.raku.org/language/101-basics#index-entry-stable_sort) built-in (available as sub and method) is stable.



Short demonstration for sorting only on the second item of each array:

```perl
use v6;
my @cities =
    ['UK', 'London'],
    ['US', 'New York'],
    ['US', 'Birmingham'],
    ['UK', 'Birmingham'],
    ;

.say for @cities.sort: { .[1] };
```
