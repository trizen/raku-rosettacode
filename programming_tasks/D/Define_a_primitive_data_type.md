[1]: https://rosettacode.org/wiki/Define_a_primitive_data_type

# [Define a primitive data type][1]

```perl
subset OneToTen of Int where 1..10;
 
my OneToTen $n = 5;
$n += 6;
```


Here the result 11 fails to smartmatch against the range `1..10`, so the second assignment throws an exception. You may use any valid smartmatch predicate within a `where` clause, so the following one-argument closure is also legal:

```perl
subset Prime of Int where -> $n { $n > 1 and so $n %% none 2 .. $n.sqrt }
```