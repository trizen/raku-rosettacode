[1]: http://rosettacode.org/wiki/Suffix_tree

# [Suffix tree][1]

Here is quite a naive algorithm, probably <img class="mwe-math-fallback-image-inline tex" alt="O(n^2)" src="http://rosettacode.org/mw/images/math/1/8/9/189317b4b935a745fcfaf95940d2b4f0.png"/>.

```perl
multi suffix-tree(Str $str) { suffix-tree map &flip, [\~] $str.flip.comb }
multi suffix-tree(@a) {
    hash
    @a == 0 ?? () !!
    @a == 1 ?? @a[0] => [] !!
    gather for @a.classify(*.substr(0, 1)) {
        my $subtree = suffix-tree(grep *.chars, map *.substr(1), .value[]);
        if $subtree == 1 {
            my $pair = $subtree.pick;
            take .key ~ $pair.key => $pair.value;
        } else {
            take .key => $subtree;
        }
    }
}
```


Displaying the tree is done with the code from [visualize a tree](http://rosettacode.org/wiki/Visualize_a_tree):

```perl
my $tree = root => suffix-tree 'banana$';
.say for visualize-tree $tree, *.key, *.value.list;
```

#### Output:
```
root
├─$
├─a
│ ├─$
│ └─na
│   ├─$
│   └─na$
├─na
│ ├─$
│ └─na$
└─banana$
```