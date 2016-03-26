[1]: http://rosettacode.org/wiki/Suffix_tree

# [Suffix tree][1]

Here is quite a naive algorithm, probably ![image](http://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=9f84a66d88d24c3b1bc91df5b5346a13&mode=mathml).

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