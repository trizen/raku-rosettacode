[1]: http://rosettacode.org/wiki/Ordered_Partitions

# [Ordered Partitions][1]

```perl
sub partition(@mask is copy) {
    my $last = [+] @mask or return [[] xx @mask];
    sort gather for @mask.kv -> $k,$v {
        next unless $v;
        temp @mask[$k] -= 1;
        for partition @mask { .take.[$k].push($last) }
    }
}
 
.perl.say for partition [2,0,2];
```

#### Output:
```
[[1, 2], [], [3, 4]]
[[1, 3], [], [2, 4]]
[[2, 3], [], [1, 4]]
[[1, 4], [], [2, 3]]
[[2, 4], [], [1, 3]]
[[3, 4], [], [1, 2]]
```