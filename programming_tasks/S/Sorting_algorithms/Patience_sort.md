[1]: https://rosettacode.org/wiki/Sorting_algorithms/Patience_sort

# [Sorting algorithms/Patience sort][1]

```perl
multi patience(*@deck) {
    my @stacks;
    for @deck -> $card {
        with @stacks.first: $card before *[*-1] -> $stack {
            $stack.push: $card;
        }
        else {
            @stacks.push: [$card];
        }
    }
    gather while @stacks {
        take .pop given min :by(*[*-1]), @stacks;
        @stacks .= grep: +*;
    }
}
 
say ~patience ^10 . pick(*);
```

#### Output:
```
0 1 2 3 4 5 6 7 8 9
```