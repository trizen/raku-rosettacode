[1]: https://rosettacode.org/wiki/Kosaraju

# [Kosaraju][1]

Inspired by Python &amp; Kotlin entries.



Accepts a hash of lists/arrays holding the vertex (name =&gt; (neighbors)) pairs. No longer limited to continuous, positive, integer vertex names.

```raku
sub kosaraju (%k) {
    my %g = %k.keys.sort Z=> flat ^%k;
    my %h = %g.invert;
    my %visited;
    my @stack;
    my @transpose;
    my @connected;
 
    sub visit ($u) {
        unless %visited{$u} {
            %visited{$u} = True;
            for |%k{$u} -> $v {
                visit($v);
                @transpose[%g{$v}].push: $u;
            }
            @stack.push: $u;
        }
    }
 
    sub assign ($u, $root) {
        if %visited{$u} {
            %visited{$u}   = False;
            @connected[%g{$u}] = $root;
            assign($_, $root) for |@transpose[%g{$u}];
        }
    }
 
    .&visit for %g.keys;
    assign($_, $_) for @stack.reverse;
 
    (|%g{@connected}).pairs.categorize( *.value, :as(*.key) ).values.map: { %h{|$_} };
}
 
# TESTING
 
-> $test { say "\nStrongly connected components: ", |kosaraju($test).sort } for
 
# Same test data as all other entries, converted to a hash of lists
(((1),(2),(0),(1,2,4),(3,5),(2,6),(5),(4,6,7)).pairs.hash),
 
# Same layout test data with named vertices instead of numbered.
(
 %(:Andy<Bart>,
   :Bart<Carl>,
   :Carl<Andy>,
   :Dave<Bart Carl Earl>,
   :Earl<Dave Fred>,
   :Fred<Carl Gary>,
   :Gary<Fred>,
   :Hank<Earl Gary Hank>)
)
```

#### Output:
```
Strongly connected components: (0 1 2)(3 4)(5 6)(7)

Strongly connected components: (Andy Bart Carl)(Dave Earl)(Fred Gary)(Hank)
```