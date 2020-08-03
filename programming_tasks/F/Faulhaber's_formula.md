[1]: https://rosettacode.org/wiki/Faulhaber's_formula

# [Faulhaber's formula][1]

```raku
sub bernoulli_number($n) {
 
    return 1/2 if $n == 1;
    return 0/1 if $n % 2;
 
    my @A;
    for 0..$n -> $m {
        @A[$m] = 1 / ($m + 1);
        for $m, $m-1 ... 1 -> $j {
            @A[$j - 1] = $j * (@A[$j - 1] - @A[$j]);
        }
    }
 
    return @A[0];
}
 
sub binomial($n, $k) {
    $k == 0 || $n == $k ?? 1 !! binomial($n-1, $k-1) + binomial($n-1, $k);
}
 
sub faulhaber_s_formula($p) {
 
    my @formula = gather for 0..$p -> $j {
        take '('
            ~ join('/', (binomial($p+1, $j) * bernoulli_number($j)).Rat.nude)
            ~ ")*n^{$p+1 - $j}";
    }
 
    my $formula = join(' + ', @formula.grep({!m{'(0/1)*'}}));
 
    $formula .= subst(rx{ '(1/1)*' }, '', :g);
    $formula .= subst(rx{ '^1'» }, '', :g);
 
    "1/{$p+1} * ($formula)";
}
 
for 0..9 -> $p {
    say "f($p) = ", faulhaber_s_formula($p);
}
```

#### Output:
```
f(0) = 1/1 * (n)
f(1) = 1/2 * (n^2 + n)
f(2) = 1/3 * (n^3 + (3/2)*n^2 + (1/2)*n)
f(3) = 1/4 * (n^4 + (2/1)*n^3 + n^2)
f(4) = 1/5 * (n^5 + (5/2)*n^4 + (5/3)*n^3 + (-1/6)*n)
f(5) = 1/6 * (n^6 + (3/1)*n^5 + (5/2)*n^4 + (-1/2)*n^2)
f(6) = 1/7 * (n^7 + (7/2)*n^6 + (7/2)*n^5 + (-7/6)*n^3 + (1/6)*n)
f(7) = 1/8 * (n^8 + (4/1)*n^7 + (14/3)*n^6 + (-7/3)*n^4 + (2/3)*n^2)
f(8) = 1/9 * (n^9 + (9/2)*n^8 + (6/1)*n^7 + (-21/5)*n^5 + (2/1)*n^3 + (-3/10)*n)
f(9) = 1/10 * (n^10 + (5/1)*n^9 + (15/2)*n^8 + (-7/1)*n^6 + (5/1)*n^4 + (-3/2)*n^2)
```