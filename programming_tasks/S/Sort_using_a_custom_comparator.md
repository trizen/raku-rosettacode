[1]: http://rosettacode.org/wiki/Sort_using_a_custom_comparator

# [Sort using a custom comparator][1]

```perl6
my @strings = <Here are some sample strings to be sorted>;
my @sorted_strings = sort { $^a.chars <=> $^b.chars or $^a.lc cmp $^b.lc }, @strings;
.say for @sorted_strings;
```


This behavior is triggered by use of an arity 2 sort routine.

```perl6
my @sorted_strings = sort -> $x { [ $x.chars, $x.lc ] }, @strings;
```