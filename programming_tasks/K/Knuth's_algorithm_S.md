[1]: https://rosettacode.org/wiki/Knuth%27s_algorithm_S

# [Knuth&#039;s algorithm S][1]



```perl
sub s_of_n_creator($n) {
    my (@sample, $i);
    -> $item {
        if    ++$i    <= $n { @sample.push:      $item }
        elsif $i.rand <  $n { @sample[$n.rand] = $item }
        @sample
    }
}

my @bin;
for ^100000 {
    my &s_of_n = s_of_n_creator 3;
    sink .&s_of_n for ^9;
    @bin[$_]++ for s_of_n 9;
}

say @bin;
```


Output:


```
29975 30028 30246 30056 30004 29983 29836 29967 29924 29981
```
