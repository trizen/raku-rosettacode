[1]: https://rosettacode.org/wiki/Averages/Mode

# [Averages/Mode][1]

```perl
sub mode (*@a) {
    my %counts := @a.Bag;
    my $max = %counts.values.max;
    return |%counts.grep(*.value == $max).map(*.key);
}
 
# Testing with arrays:
say mode [1, 3, 6, 6, 6, 6, 7, 7, 12, 12, 17];
say mode [1, 1, 2, 4, 4];
```

#### Output:
```
6
(4 1)
```


Alternatively, a version that uses a single method chain with no temporary variables: (Same output with same input)

```perl
sub mode (*@a) {
    return |(@a
        .Bag                # count elements
        .classify(*.value)  # group elements with the same count
        .max(*.key)         # get group with the highest count
        .value.map(*.key);  # get elements in the group
    );
}
 
say mode [1, 3, 6, 6, 6, 6, 7, 7, 12, 12, 17];
say mode [1, 1, 2, 4, 4];
```