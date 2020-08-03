[1]: https://rosettacode.org/wiki/Arithmetic-geometric_mean/Calculate_Pi

# [Arithmetic-geometric mean/Calculate Pi][1]

There is not yet a FixDecimal type module in Perl 6, and using FatRat all along would be too slow and would be coerced to Num when computing the square root anyway, so we'll use a custom definition of the square root for Int and FatRat, with a limitation to the number of decimals. We'll show all the intermediate results.



The trick to compute the square root of a rational ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=f840899ae807a7c043751f8529403bae&mode=mathml) up to a certain amount of decimals N is to write:



![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=7e8ff6a27540ed22f0e6bd1b2f7285f1&mode=mathml)



so that what we need is one square root of a big number that we'll truncate to its integer part. We'll compute the square root of this big integer by using the convergence of the recursive sequence:



![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=2a9aefca4f9c8ffa48a812145167786e&mode=mathml)



It's not too hard to see that such a sequence converges towards ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=f108a3d88b22ff91ddbd459b0f359bc9&mode=mathml).



Notice that we don't get the exact number of decimals required&#160;: the last two decimals or so can be wrong. This is because we don't need ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=825b3fd5bafbc46b9a560ea9f16b21dd&mode=mathml), but rather ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=2864884e3a76f4b27e189d49f4e6d0d1&mode=mathml). Elevating to the square makes us lose a bit of precision. It could be compensated by choosing a slightly higher value of N (in a way that could be precisely calculated), but that would probably be overkill.

```raku
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