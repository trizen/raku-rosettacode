[1]: https://rosettacode.org/wiki/Detect_division_by_zero

# [Detect division by zero][1]





#### Try/Catch

```perl
sub div($a, $b) {
    my $r;
    try {
        $r = $a / $b;
        CATCH {
            default { note "Unexpected exception, $_" }
        }
    }
    return $r // Nil;
}
say div(10,2);
say div(1, sin(0));
```

#### Output:
```
5
Unexpected exception, Attempt to divide 1 by zero using /
Nil
```


#### Multi Method Dispatch

```perl
multi div($a, $b) { return $a / $b }
multi div($a, $b where { $b == 0 }) { note 'Attempt to divide by zero.'; return Nil }

say div(10, 2);
say div(1, sin(0));
```

#### Output:
```
5
Attempt to divide by zero.
Nil
```
