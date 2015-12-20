[1]: http://rosettacode.org/wiki/Roots_of_a_function

# [Roots of a function][1]

Uses exact arithmetic.

```perl6
sub f($x) { $x*$x*$x - 3*$x*$x + 2*$x }
 
my $start = -1;
my $stop = 3;
my $step = 0.001;
 
for $start, * + $step ... $stop -> $x {
    state $sign = 0;
    given f($x) {
        my $next = .sign;
        when 0.0 {
            say "Root found at $x";
        }
        when $sign and $next != $sign {
            say "Root found near $x";
        }
        NEXT $sign = $next;
    }
}
```

#### Output:
```
Root found at 0
Root found at 1
Root found at 2
```