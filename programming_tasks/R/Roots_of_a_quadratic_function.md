[1]: http://rosettacode.org/wiki/Roots_of_a_quadratic_function

# [Roots of a quadratic function][1]

Perl 6 has complex number handling built in.

```perl6
my @sets = [1, 2, 1],
           [1, 2, 3],
           [1, -2, 1],
           [1, 0, -4],
           [1, -10**6, 1];
 
for @sets -> @coefficients {
    say "Roots for @coefficients.join(', ').fmt("%-16s")",
        "=> (&quadroots( @coefficients ).join(', '))";
}
 
multi sub quadroots ($a, $b, $c) {
    ( -$b + $_ ) / (2 * $a),
    ( -$b - $_ ) / (2 * $a) 
    given
    ($b ** 2 - 4 * $a * $c).Complex.sqrt.narrow
}
 
multi sub quadroots (@a) {
    @a == 3 or die "Expected three elements, got {+@a}";
    quadroots |@a;
}
```

#### Output:
```
Roots for 1, 2, 1         => (-1, -1)
Roots for 1, 2, 3         => (-1+1.4142135623731i, -1-1.4142135623731i)
Roots for 1, -2, 1        => (1, 1)
Roots for 1, 0, -4        => (2, -2)
Roots for 1, -1000000, 1  => (999999.999999, 1.00000761449337e-06)
```