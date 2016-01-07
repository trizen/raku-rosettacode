[1]: http://rosettacode.org/wiki/Array_length

# [Array length][1]

To get the number of elements of an array in Perl 6 you put the array in a coercing Numeric context, or call `elems` on it.

```perl
 
my @array = <apple orange banana>;
 
say @array.elems;  # 3
say elems @array;  # 3
say +@array;       # 3
say @array + 1;    # 4
 
```


Watch out for infinite/lazy arrays though. You can't get the length of those.

```perl
my @infinite = 1 .. Inf;  # 1, 2, 3, 4, ...
 
say @infinite[5000];  # 5001
say @infinite.elems;  # Throws exception "Cannot .elems a lazy list"
 
```