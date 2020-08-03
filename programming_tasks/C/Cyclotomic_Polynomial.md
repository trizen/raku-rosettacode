[1]: https://rosettacode.org/wiki/Cyclotomic_Polynomial

# [Cyclotomic Polynomial][1]

Uses the same library as Perl, so comes with the same caveats.

```raku
use Math::Polynomial::Cyclotomic:from<Perl5> <cyclo_poly_iterate cyclo_poly>;
 
say 'First 30 cyclotomic polynomials:';
my $iterator = cyclo_poly_iterate(1);
say "Φ($_) = " ~ super $iterator().Str for 1..30;
 
say "\nSmallest cyclotomic polynomial with |n| as a coefficient:";
say "Φ(1) has a coefficient magnitude: 1";
 
my $index = 0;
for 2..9 -> $coefficient {
    loop {
        $index += 5;
        my \Φ = cyclo_poly($index);
        next unless Φ ~~ / $coefficient\* /;
        say "Φ($index) has a coefficient magnitude: $coefficient";
        $index -= 5;
        last;
    }
}
 
sub super ($str) {
    $str.subst( / '^' (\d+) /, { $0.trans([<0123456789>.comb] => [<⁰¹²³⁴⁵⁶⁷⁸⁹>.comb]) }, :g)
}
```

#### Output:
```
First 30 cyclotomic polynomials:
Φ(1) = (x - 1)
Φ(2) = (x + 1)
Φ(3) = (x² + x + 1)
Φ(4) = (x² + 1)
Φ(5) = (x⁴ + x³ + x² + x + 1)
Φ(6) = (x² - x + 1)
Φ(7) = (x⁶ + x⁵ + x⁴ + x³ + x² + x + 1)
Φ(8) = (x⁴ + 1)
Φ(9) = (x⁶ + x³ + 1)
Φ(10) = (x⁴ - x³ + x² - x + 1)
Φ(11) = (x¹⁰ + x⁹ + x⁸ + x⁷ + x⁶ + x⁵ + x⁴ + x³ + x² + x + 1)
Φ(12) = (x⁴ - x² + 1)
Φ(13) = (x¹² + x¹¹ + x¹⁰ + x⁹ + x⁸ + x⁷ + x⁶ + x⁵ + x⁴ + x³ + x² + x + 1)
Φ(14) = (x⁶ - x⁵ + x⁴ - x³ + x² - x + 1)
Φ(15) = (x⁸ - x⁷ + x⁵ - x⁴ + x³ - x + 1)
Φ(16) = (x⁸ + 1)
Φ(17) = (x¹⁶ + x¹⁵ + x¹⁴ + x¹³ + x¹² + x¹¹ + x¹⁰ + x⁹ + x⁸ + x⁷ + x⁶ + x⁵ + x⁴ + x³ + x² + x + 1)
Φ(18) = (x⁶ - x³ + 1)
Φ(19) = (x¹⁸ + x¹⁷ + x¹⁶ + x¹⁵ + x¹⁴ + x¹³ + x¹² + x¹¹ + x¹⁰ + x⁹ + x⁸ + x⁷ + x⁶ + x⁵ + x⁴ + x³ + x² + x + 1)
Φ(20) = (x⁸ - x⁶ + x⁴ - x² + 1)
Φ(21) = (x¹² - x¹¹ + x⁹ - x⁸ + x⁶ - x⁴ + x³ - x + 1)
Φ(22) = (x¹⁰ - x⁹ + x⁸ - x⁷ + x⁶ - x⁵ + x⁴ - x³ + x² - x + 1)
Φ(23) = (x²² + x²¹ + x²⁰ + x¹⁹ + x¹⁸ + x¹⁷ + x¹⁶ + x¹⁵ + x¹⁴ + x¹³ + x¹² + x¹¹ + x¹⁰ + x⁹ + x⁸ + x⁷ + x⁶ + x⁵ + x⁴ + x³ + x² + x + 1)
Φ(24) = (x⁸ - x⁴ + 1)
Φ(25) = (x²⁰ + x¹⁵ + x¹⁰ + x⁵ + 1)
Φ(26) = (x¹² - x¹¹ + x¹⁰ - x⁹ + x⁸ - x⁷ + x⁶ - x⁵ + x⁴ - x³ + x² - x + 1)
Φ(27) = (x¹⁸ + x⁹ + 1)
Φ(28) = (x¹² - x¹⁰ + x⁸ - x⁶ + x⁴ - x² + 1)
Φ(29) = (x²⁸ + x²⁷ + x²⁶ + x²⁵ + x²⁴ + x²³ + x²² + x²¹ + x²⁰ + x¹⁹ + x¹⁸ + x¹⁷ + x¹⁶ + x¹⁵ + x¹⁴ + x¹³ + x¹² + x¹¹ + x¹⁰ + x⁹ + x⁸ + x⁷ + x⁶ + x⁵ + x⁴ + x³ + x² + x + 1)
Φ(30) = (x⁸ + x⁷ - x⁵ - x⁴ - x³ + x + 1)

Smallest cyclotomic polynomial with |n| as a coefficient:
Φ(1) has a coefficient magnitude: 1
Φ(105) has a coefficient magnitude: 2
Φ(385) has a coefficient magnitude: 3
Φ(1365) has a coefficient magnitude: 4
Φ(1785) has a coefficient magnitude: 5
Φ(2805) has a coefficient magnitude: 6
Φ(3135) has a coefficient magnitude: 7
Φ(6545) has a coefficient magnitude: 8
Φ(6545) has a coefficient magnitude: 9
```
