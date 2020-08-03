[1]: https://rosettacode.org/wiki/Tree_traversal

# [Tree traversal][1]

```perl
class TreeNode {
    has TreeNode $.parent;
    has TreeNode $.left;
    has TreeNode $.right;
    has $.value;
 
    method pre-order {
        flat gather {
            take $.value;
            take $.left.pre-order if $.left;
            take $.right.pre-order if $.right
        }
    }
 
    method in-order {
        flat gather {
            take $.left.in-order if $.left;
            take $.value;
            take $.right.in-order if $.right;
        }
    }
 
    method post-order {
        flat gather {
            take $.left.post-order if $.left;
            take $.right.post-order if $.right;
            take $.value;
        }
    }
 
    method level-order {
        my TreeNode @queue = (self);
        flat gather while @queue.elems {
            my $n = @queue.shift;
            take $n.value;
            @queue.push($n.left) if $n.left;
            @queue.push($n.right) if $n.right;
        }
    }
}
 
my TreeNode $root .= new( value => 1,
                    left => TreeNode.new( value => 2,
                            left => TreeNode.new( value => 4, left => TreeNode.new(value => 7)),
                            right => TreeNode.new( value => 5)
                    ),
                    right => TreeNode.new( value => 3, 
                             left => TreeNode.new( value => 6, 
                                     left => TreeNode.new(value => 8),
                                     right => TreeNode.new(value => 9)
                                     )
                             )
                    );
 
say "preorder:  ",$root.pre-order.join(" ");
say "inorder:   ",$root.in-order.join(" ");
say "postorder: ",$root.post-order.join(" ");
say "levelorder:",$root.level-order.join(" ");
```

#### Output:
```
preorder:  1 2 4 7 5 3 6 8 9
inorder:   7 4 2 5 1 8 6 9 3
postorder: 7 4 5 2 8 9 6 3 1
levelorder:1 2 3 4 5 6 7 8 9
```