[1]: http://rosettacode.org/wiki/Same_Fringe

# [Same Fringe][1]

Unlike in Perl 5, where `=&gt;` is just a synonym for comma, in Perl 6 it creates a true Pair object. So here we use Pair objects for our "cons" cells, just as if we were doing this in Lisp. We use the `gather/take` construct to harvest the leaves lazily as the elements are visited with a standard recursive algorithm, using multiple dispatch to differentiate nodes from leaves. The `eqv` value equivalence is applied to the two lists in parallel.

```perl
sub fringe ($tree) {
    multi sub fringey (Pair $node) { fringey $_ for $node.kv; }
    multi sub fringey ( Any $leaf) { take $leaf; }
 
    gather fringey $tree;
}
 
sub samefringe ($a, $b) { fringe($a) eqv fringe($b) }
```


Testing:

```perl
my $a = 1 => 2 => 3 => 4 => 5 => 6 => 7 => 8;
my $b = 1 => (( 2 => 3 ) => (4 => (5 => ((6 => 7) => 8))));
my $c = (((1 => 2) => 3) => 4) => 5 => 6 => 7 => 8;
 
my $x = 1 => 2 => 3 => 4 => 5 => 6 => 7 => 8 => 9;
my $y = 0 => 2 => 3 => 4 => 5 => 6 => 7 => 8;
my $z = 1 => 2 => (4 => 3) => 5 => 6 => 7 => 8;
 
say  so samefringe $a, $a;
say  so samefringe $a, $b;
say  so samefringe $a, $c;
 
say not samefringe $a, $x;
say not samefringe $a, $y;
say not samefringe $a, $z;
```

#### Output:
```
True
True
True
True
True
True
```