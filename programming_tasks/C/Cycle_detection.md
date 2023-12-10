[1]: https://rosettacode.org/wiki/Cycle_detection

# [Cycle detection][1]





Pretty much a line for line translation of the Python code on the Wikipedia page.

```perl
sub cyclical-function (\x) { (x * x + 1) % 255 };

my ( $l, $s ) = brent( &cyclical-function, 3 );

put join ', ', (3, -> \x { cyclical-function(x) } ... *)[^20], '...';
say "Cycle length $l.";
say "Cycle start index $s.";
say (3, -> \x { cyclical-function(x) } ... *)[$s .. $s + $l - 1];

sub brent (&f, $x0) {
    my $power = my $λ = 1;
    my $tortoise = $x0;
    my $hare = f($x0);
    while ($tortoise != $hare) {
        if $power == $λ {
            $tortoise = $hare;
            $power *= 2;
            $λ = 0;
        }
        $hare = f($hare);
        $λ += 1;
    }

    my $μ = 0;
    $tortoise = $hare = $x0;
    $hare = f($hare) for ^$λ;

    while ($tortoise != $hare) {
        $tortoise = f($tortoise);
        $hare = f($hare);
        $μ += 1;
    }
    return $λ, $μ;
}
```

#### Output:
```
3, 10, 101, 2, 5, 26, 167, 95, 101, 2, 5, 26, 167, 95, 101, 2, 5, 26, 167, 95, ...
Cycle length 6.
Cycle start index 2.
(101 2 5 26 167 95)
```
