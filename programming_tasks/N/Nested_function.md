[1]: https://rosettacode.org/wiki/Nested_function

# [Nested function][1]



```perl
sub make-List ($separator = ') '){
    my $count = 1;

    sub make-Item ($item) { "{$count++}$separator$item" }

    join "\n", <first second third>».&make-Item;
}

put make-List('. ');
```

#### Output:
```
1. first
2. second
3. third
```
