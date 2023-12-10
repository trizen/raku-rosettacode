[1]: https://rosettacode.org/wiki/Array_length

# [Array length][1]





To get the number of elements of an array in Raku you put the array in a coercing Numeric context, or call `elems` on it.

```perl
my @array = <apple orange>;

say @array.elems;  # 2
say elems @array;  # 2
say + @array;      # 2
say @array + 0;    # 2
```


Watch out for infinite/lazy arrays though. You can't get the length of those.

```perl
my @infinite = 1 .. Inf;  # 1, 2, 3, 4, ...

say @infinite[5000];  # 5001
say @infinite.elems;  # Throws exception "Cannot .elems a lazy list"
```
