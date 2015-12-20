[1]: http://rosettacode.org/wiki/Sorting_algorithms/Bead_sort

# [Sorting algorithms/Bead sort][1]

```perl6
use List::Utils;
 
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


Here we simulate the dropping beads by using the <tt>push</tt> method.

```perl6
sub beadsort(*@list) {
    my @rods;
    for ^«@list -> $x { @rods[$x].push(1) }
    gather for ^@rods[0] -> $y {
        take [+] @rods.map: { .[$y] // last }
    }
}
 
say beadsort 2,1,3,5;
```


The <tt>^</tt> is the "upto" operator that gives a range of 0 up to (but not including) its endpoint. We use it as a hyperoperator (<tt>^«</tt>) to generate all the ranges of rod numbers we should drop a bead on, with the result that <tt>$x</tt> tells us which rod to drop each bead on. Then we use <tt>^</tt> again on the first rod to see how deep the beads are stacked, since they are guaranteed to be the deepest there. The <tt>[+]</tt> adds up all the beads that are found at level <tt>$y</tt>. The <tt>last</tt> short circuits the map so we don't have to look for all the missing beads at a given level, since the missing beads are all guaranteed to come after the existing beads at that level (because we always dropped left to right starting at rod 0).