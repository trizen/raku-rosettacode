[1]: http://rosettacode.org/wiki/Short-circuit_evaluation

# [Short-circuit evaluation][1]

```perl
sub a ($p) { print 'a'; $p }
sub b ($p) { print 'b'; $p }
 
for '&&', '||' -> $op {
    for True, False X True, False -> $p, $q {
        my $s = "a($p) $op b($q)";
        print "$s: ";
        eval $s;
        print "\n";
    }
}
```

#### Output:
```
a(1) && b(1): ab
a(1) && b(0): ab
a(0) && b(1): a
a(0) && b(0): a
a(1) || b(1): a
a(1) || b(0): a
a(0) || b(1): ab
a(0) || b(0): ab
```