[1]: http://rosettacode.org/wiki/Suffix_tree

# [Suffix tree][1]

Here is quite a naive algorithm, probably <span class="texhtml" dir="ltr">_O_(_n_<sup>2</sup>)</span>.

```perl
multi suffix-tree(Str $str) { suffix-tree [map](http://perldoc.perl.org/functions/map.html) &flip, [\~] $str.flip.comb }
multi suffix-tree(@a) {
    hash
    @a == 0 ?? () !!
    @a == 1 ?? @a[0] => [] !!
    gather for @a.classify(*.[substr](http://perldoc.perl.org/functions/substr.html)(0, 1)) {
        my $subtree = suffix-tree([grep](http://perldoc.perl.org/functions/grep.html) *.chars, [map](http://perldoc.perl.org/functions/map.html) *.[substr](http://perldoc.perl.org/functions/substr.html)(1), .value[]);
        if $subtree == 1 {
            my $pair = $subtree.pick;
            take .key ~ $pair.key => $pair.value;
        } else {
            take .key => $subtree;
        }
    }
}
```


Displaying the tree is done with the code from [visualize a tree](/wiki/Visualize\_a\_tree" title="Visualize a tree):

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