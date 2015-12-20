[1]: http://rosettacode.org/wiki/Arithmetic-geometric_mean/Calculate_Pi

# [Arithmetic-geometric mean/Calculate Pi][1]

There is not yet a FixDecimal type module in Perl 6, and using FatRat all along would be too slow and would be coerced to Num when computing the square root anyway, so we'll use a custom definition of the square root for Int and FatRat, with a limitation to the number of decimals. We'll show all the intermediate results.



The trick to compute the square root of a rational <img class="tex" alt="n\over d" src="/mw/images/math/7/b/0/7b0364285593c3f0b4f1547e879b7a30.png"/> up to a certain amount of decimals N is to write:



<img class="tex" alt="\sqrt{\frac{n}{d}} = \sqrt{&#10;\frac{n 10^{2N} / d}{d 10^{2N} / d}&#10;} = \frac{\sqrt{n 10^{2N} / d}}{10^N}" src="/mw/images/math/f/c/4/fc4a3d254c7dd00711e6cba90a07bb9b.png"/>



so that what we need is one square root of a big number that we'll truncate to its integer part. We'll compute the square root of this big integer by using the convergence of the recursive sequence:



<img class="tex" alt="u\_{n+1} = \frac{1}{2}(u\_n + \frac{x}{u\_n})" src="/mw/images/math/e/f/1/ef1b37a4a4a2552a574dd4354a3f159e.png"/>



It's not too hard to see that such a sequence converges towards <img class="tex" alt="\sqrt x" src="/mw/images/math/a/0/b/a0b9673c7c97664405abeea23b78087a.png"/>.



Notice that we don't get the exact number of decimals required&#160;: the last two decimals or so can be wrong. This is because we don't need <span class="texhtml" dir="ltr">_a_<sub>_n_</sub></span>, but rather <img class="tex" alt="a\_n^2" src="/mw/images/math/6/a/2/6a2ded63ebfb3deb8d76656e00d56e44.png"/>. Elevating to the square makes us lose a bit of precision. It could be compensated by choosing a slightly higher value of N (in a way that could be precisely calculated), but that would probably be overkill.

```perl
constant number-of-decimals = 100;
 
multi sqrt(Int $n) {
    .[*-1] given
    1, { ($_ + $n div $_) div 2 } ... * == *
}
multi sqrt(FatRat $r --> FatRat) {
    return FatRat.new:
    sqrt($r.nude[0] * 10**(number-of-decimals*2) div $r.nude[1]),
    10**number-of-decimals;
}
 
my FatRat ($a, $n) = 1.FatRat xx 2;
my FatRat $g = sqrt(1/2.FatRat);
my $z = .25;
 
for ^10 {
    given [ ($a + $g)/2, sqrt($a * $g) ] {
	$z -= (.[0] - $a)**2 * $n;
	$n += $n;
	($a, $g) = @$_;
	say ($a ** 2 / $z).substr: 0, 2 + number-of-decimals;
    }
}
```

#### Output:
```
3.1876726427121086272019299705253692326510535718593692264876339862751228325281223301147286106601617972
3.1416802932976532939180704245600093827957194388154028326441894631956630010102553193888894275152646100
3.1415926538954464960029147588180434861088792372613115896511013576846530795030865017740975862898631567
3.1415926535897932384663606027066313217577024113424293564868460152384109486069277582680622007332762125
3.1415926535897932384626433832795028841971699491647266058346961259487480060953290058518515759317101932
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280468522286541140
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170668
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170665
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170664
3.1415926535897932384626433832795028841971693993751058209749445923078164062862089986280348253421170663
```