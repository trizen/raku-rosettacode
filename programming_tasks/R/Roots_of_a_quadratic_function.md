[1]: https://rosettacode.org/wiki/Roots_of_a_quadratic_function

# [Roots of a quadratic function][1]

Perl 6 has complex number handling built in.

```perl
for
[1, 2, 1],
[1, 2, 3],
[1, -2, 1],
[1, 0, -4],
[1, -10**6, 1]
-> @coefficients {
    printf "Roots for %d, %d, %d\t=> (%s, %s)\n",
    |@coefficients, |quadroots(@coefficients);
}
 
sub quadroots (*[$a, $b, $c]) {
    ( -$b + $_ ) / (2 * $a),
    ( -$b - $_ ) / (2 * $a) 
    given
    ($b ** 2 - 4 * $a * $c ).Complex.sqrt.narrow
}
```

#### Output:
```
Roots for 1, 2, 1       => (-1, -1)
Roots for 1, 2, 3       => (-1+1.4142135623731i, -1-1.4142135623731i)
Roots for 1, -2, 1      => (1, 1)
Roots for 1, 0, -4      => (2, -2)
Roots for 1, -1000000, 1        => (999999.999999, 1.00000761449337e-06)
```