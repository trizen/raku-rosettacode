[1]: https://rosettacode.org/wiki/Ordered_partitions

# [Ordered partitions][1]



```perl
sub partition(@mask is copy) {
    my @op;
    my $last = [+] @mask or return [] xx 1;
    for @mask.kv -> $k, $v {
        next unless $v;
        temp @mask[$k] -= 1;
        for partition @mask -> @p {
            @p[$k].push: $last;
            @op.push: @p;
        }
    }
    return @op;
}
 
.say for reverse partition [2,0,2];
```

#### Output:
```
[[1, 2], (Any), [3, 4]]
[[1, 3], (Any), [2, 4]]
[[2, 3], (Any), [1, 4]]
[[1, 4], (Any), [2, 3]]
[[2, 4], (Any), [1, 3]]
[[3, 4], (Any), [1, 2]]
```
