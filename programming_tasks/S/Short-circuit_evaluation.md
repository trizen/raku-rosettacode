[1]: https://rosettacode.org/wiki/Short-circuit_evaluation

# [Short-circuit evaluation][1]



```perl
use MONKEY-SEE-NO-EVAL;

sub a ($p) { print 'a'; $p }
sub b ($p) { print 'b'; $p }

for 1, 0 X 1, 0 -> ($p, $q) {
    for '&&', '||' -> $op {
        my $s = "a($p) $op b($q)";
        print "$s: ";
        EVAL $s;
        print "\n";
    }
}
```

#### Output:
```
a(1) && b(1): ab
a(1) || b(1): a
a(1) && b(0): ab
a(1) || b(0): a
a(0) && b(1): a
a(0) || b(1): ab
a(0) && b(0): a
a(0) || b(0): ab
```
