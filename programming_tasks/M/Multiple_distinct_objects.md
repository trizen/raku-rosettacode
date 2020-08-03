[1]: https://rosettacode.org/wiki/Multiple_distinct_objects

# [Multiple distinct objects][1]

Unlike in Perl 5, the list repetition operator evaluates the left argument thunk each time, so

```raku
my @a = Foo.new xx $n;
```


produces `$n` distinct objects.