[1]: https://rosettacode.org/wiki/Sorting_algorithms/Bead_sort

# [Sorting algorithms/Bead sort][1]

```perl
# routine cribbed from List::Utils;
sub transpose(@list is copy) {
    gather {
        while @list {
            my @heads;
            if @list[0] !~~ Positional { @heads = @list.shift; }
            else { @heads = @list.map({$_.shift unless $_ ~~ []}); }
            @list = @list.map({$_ unless $_ ~~ []});
            take [@heads];
        }
    }
}
 
sub beadsort(@l) {
    (transpose(transpose(map {[1 xx $_]}, @l))).map(*.elems);
}
 
my @list = 2,1,3,5;
say beadsort(@list).perl;
```

#### Output:
```
(5, 3, 2, 1)
```


Here we simulate the dropping beads by using the `push` method.

```perl
sub beadsort(*@list) {
    my @rods;
    for words ^«@list -> $x { @rods[$x].push(1) }
    gather for ^@rods[0] -> $y {
        take [+] @rods.map: { .[$y] // last }
    }
}
 
say beadsort 2,1,3,5;
```


The `^` is the "upto" operator that gives a range of 0 up to (but not including) its endpoint. We use it as a hyperoperator (`^«`) to generate all the ranges of rod numbers we should drop a bead on, with the result that `$x` tells us which rod to drop each bead on. Then we use `^` again on the first rod to see how deep the beads are stacked, since they are guaranteed to be the deepest there. The `[+]` adds up all the beads that are found at level `$y`. The `last` short circuits the map so we don't have to look for all the missing beads at a given level, since the missing beads are all guaranteed to come after the existing beads at that level (because we always dropped left to right starting at rod 0).