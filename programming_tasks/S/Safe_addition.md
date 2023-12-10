[1]: https://rosettacode.org/wiki/Safe_addition

# [Safe addition][1]





Raku uses 64 bit IEEE floating points numbers which  provide 53 binary digits
of accuracy. If you insist on using floats your answer will be accurate in the range
± 1.1102230246251565e-16. Raku actually makes it somewhat difficult to output
an exact stringified float with more than 15 digits of accuracy. It automatically
rounds since it doesn't try to pretend that it is more accurate than that.



If you really DO need very high precision and accuracy, Raku has native
built-in Rational number support. Rationals are the ratio of two integers, and
are always exact. Standard Rats are limited to 64 bits of precision in the
denominator. On overflow, they will downgrade to a float (Num).
FatRats have unlimited denominators.



There is no need to load any external libraries or force the use of Rats. Unless
you force it otherwise, decimal number are always stored as Rats as long as they
will fit within the denominator limit.



You can see the two integers making up the ratio of a Rat (or FatRat) using the
.nude method (**nu**merator, **de**nominator).



Rats and FatRats by default stringify to a fixed number of places for
non-terminating fractions and the same number of digits as the denominator for
terminating fractions. If you need more control over stringification, load the
module Rat::Precise for configurable stringification. Rat::Precise *only* affects
stringification, not calculation. Calculations are always done at full (exact) precision.






```perl
say "Floating points: (Nums)";
say "Error: " ~ (2**-53).Num;

sub infix:<±+> (Num $a, Num $b) {
    my \ε = (2**-53).Num;
    $a - ε + $b, $a + ε + $b,
}

printf "%4.16f .. %4.16f\n", (1.14e0 ±+ 2e3);

say "\nRationals:";

say ".1 + .2 is exactly equal to .3: ", .1 + .2 === .3;

say "\nLarge denominators require explicit coercion to FatRats:";
say "Sum of inverses of the first 500 natural numbers:";
my $sum = sum (1..500).map: { FatRat.new(1,$_) };
say $sum;
say $sum.nude;

{
    say "\nRat stringification may not show full precision for terminating fractions by default.";
    say "Use a module to get full precision.";
    use Rat::Precise; # module loading is scoped to the enclosing block
    my $rat = 1.5**63;
    say "\nRaku default stringification for 1.5**63:\n" ~ $rat; # standard stringification
    say "\nRat::Precise stringification for 1.5**63:\n" ~$rat.precise; # full precision
}
```

#### Output:
```
Floating points: (Nums)
Error: 1.1102230246251565e-16
2001.1400000000000000 .. 2001.1400000000000000

Rationals:
.1 + .2 is exactly equal to .3: True

Large denominators require explicit coercion to FatRats:
Sum of inverses of the first 500 natural numbers:
6.79282342999052460298928714536797369481981381439677911664308889685435662379055049245764940373586560391756598584375065928223134688479711715030249848314807266844371012370203147722209400570479644295921001097190193214586271
(6633384299891198065461433023874214660151383488987829406868700907802279376986364154005690172480537248349310365871218591743641116766728139494727637850449054802989613344274500453825922847052235859615378238909694581687099 976528297586037954584851660253897317730151176683856787284655867127950765610716178591036797598551026470244168088645451676177520177514977827924165875515464044694152207479405310883385229609852607806002629415184926954240)

Rat stringification may not show full precision for terminating fractions by default.
Use a module to get full precision.

Raku default stringification for 1.5**63:
124093581919.64894769782737365038

Rat::Precise stringification for 1.5**63:
124093581919.648947697827373650380188008224280338254175148904323577880859375
```
