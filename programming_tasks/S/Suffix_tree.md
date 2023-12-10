[1]: https://rosettacode.org/wiki/Suffix_tree

# [Suffix tree][1]





Here is quite a naive algorithm, probably <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle O(n^{2})}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>O</mi>
        <mo stretchy="false">(</mo>
        <msup>
          <mi>n</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>2</mn>
          </mrow>
        </msup>
        <mo stretchy="false">)</mo>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle O(n^{2})}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/6cd9594a16cb898b8f2a2dff9227a385ec183392" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:6.032ex; height:3.176ex;" alt="{\displaystyle O(n^2)}"></span>.



The display code is a variant of the [visualize a tree](https://rosettacode.org/wiki/Visualize_a_tree#Raku) task code.

```perl
multi suffix-tree(Str $str) { suffix-tree flat map &flip, [\~] $str.flip.comb }
multi suffix-tree(@a) {
    hash
    @a == 0 ?? () !!
    @a == 1 ?? ( @a[0] => [] ) !!
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

my $tree = root => suffix-tree 'banana$';

.say for visualize-tree $tree, *.key, *.value.List;

sub visualize-tree($tree, &label, &children,
                   :$indent = '',
                   :@mid = ('├─', '│ '),
                   :@end = ('└─', '  '),
) {
    sub visit($node, *@pre) {
        gather {
            take @pre[0] ~ $node.&label;
            my @children = sort $node.&children;
            my $end = @children.end;
            for @children.kv -> $_, $child {
                when $end { take visit($child, (@pre[1] X~ @end)) }
                default   { take visit($child, (@pre[1] X~ @mid)) }
            }
        }
    }
    flat visit($tree, $indent xx 2);
}
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
├─banana$
└─na
  ├─$
  └─na$
```
