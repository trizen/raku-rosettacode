[1]: http://rosettacode.org/wiki/Knuth's_algorithm_S

# [Knuth's algorithm S][1]

```perl6
sub s_of_n_creator($n) {
    my @sample;
    my $i = 0;
    -> $item {
        if ++$i <= $n {
            push @sample, $item;
        }
        elsif $i.rand < $n {
            @sample[$n.rand] = $item;
        }
        @sample;
    }
}
 
my @items = 0..9;
my @bin;
 
for ^100000 {
    my &s_of_n = s_of_n_creator(3);
    my @sample;
    for @items -> $item {
        @sample = s_of_n($item);
    }
    for @sample -> $s {
        @bin[$s]++;
    }
}
say @bin;
```


Output:


#### Output:
```
29975 30028 30246 30056 30004 29983 29836 29967 29924 29981
```