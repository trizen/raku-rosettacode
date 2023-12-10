[1]: https://rosettacode.org/wiki/Sort_using_a_custom_comparator

# [Sort using a custom comparator][1]



Primary sort by length of string, then break ties by sorting alphabetically (ignoring case).

```perl
my @strings = <Here are some sample strings to be sorted>;
put @strings.sort:{.chars, .lc};
put sort -> $x { $x.chars, $x.lc }, @strings;
```

#### Output:
```
be to are Here some sample sorted strings
be to are Here some sample sorted strings
```
