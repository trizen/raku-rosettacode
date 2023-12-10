[1]: https://rosettacode.org/wiki/Fermat_numbers

# [Fermat numbers][1]





I gave up on factoring F₉ after about 20 minutes.

```perl
use ntheory:from<Perl5> <factor>;

my @Fermats = (^Inf).map: 2 ** 2 ** * + 1;

my $sub = '₀';
say "First 10 Fermat numbers:";
printf "F%s = %s\n", $sub++, $_ for @Fermats[^10];

$sub = '₀';
say "\nFactors of first few Fermat numbers:";
for @Fermats[^9].map( {"$_".&factor} ) -> $f {
    printf "Factors of F%s: %s %s\n", $sub++, $f.join(' '), $f.elems == 1 ?? '- prime' !! ''
}
```

#### Output:
```
First 10 Fermat numbers:
F₀ = 3
F₁ = 5
F₂ = 17
F₃ = 257
F₄ = 65537
F₅ = 4294967297
F₆ = 18446744073709551617
F₇ = 340282366920938463463374607431768211457
F₈ = 115792089237316195423570985008687907853269984665640564039457584007913129639937
F₉ = 13407807929942597099574024998205846127479365820592393377723561443721764030073546976801874298166903427690031858186486050853753882811946569946433649006084097

Factors of first few Fermat numbers:
Factors of F₀: 3 - prime
Factors of F₁: 5 - prime
Factors of F₂: 17 - prime
Factors of F₃: 257 - prime
Factors of F₄: 65537 - prime
Factors of F₅: 641 6700417 
Factors of F₆: 274177 67280421310721 
Factors of F₇: 59649589127497217 5704689200685129054721 
Factors of F₈: 1238926361552897 93461639715357977769163558199606896584051237541638188580280321
```
