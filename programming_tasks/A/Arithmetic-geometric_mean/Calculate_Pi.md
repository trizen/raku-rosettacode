[1]: https://rosettacode.org/wiki/Arithmetic-geometric_mean/Calculate_Pi

# [Arithmetic-geometric mean/Calculate Pi][1]





There is not yet a FixDecimal type module in Raku, and using FatRat all along would be too slow and would be coerced to Num when computing the square root anyway, so we'll use a custom definition of the square root for Int and FatRat, with a limitation to the number of decimals.  We'll show all the intermediate results.



The trick to compute the square root of a rational <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle n \over d}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mfrac>
        <mstyle displaystyle="true" scriptlevel="0">
          <mi>n</mi>
        </mstyle>
        <mi>d</mi>
      </mfrac>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle n \over d}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/f3c696049051dc8aca901f540eecc885951a5e1a" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -2.005ex; width:2.231ex; height:4.843ex;" alt="{\displaystyle n\over d}"></span> up to a certain amount of decimals N is to write:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle {\sqrt {\frac {n}{d}}}={\sqrt {\frac {n10^{2N}/d}{d10^{2N}/d}}}={\frac {\sqrt {n10^{2N}/d}}{10^{N}}}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mrow class="MJX-TeXAtom-ORD">
          <msqrt>
            <mfrac>
              <mi>n</mi>
              <mi>d</mi>
            </mfrac>
          </msqrt>
        </mrow>
        <mo>=</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <msqrt>
            <mfrac>
              <mrow>
                <mi>n</mi>
                <msup>
                  <mn>10</mn>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                    <mi>N</mi>
                  </mrow>
                </msup>
                <mrow class="MJX-TeXAtom-ORD">
                  <mo>/</mo>
                </mrow>
                <mi>d</mi>
              </mrow>
              <mrow>
                <mi>d</mi>
                <msup>
                  <mn>10</mn>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                    <mi>N</mi>
                  </mrow>
                </msup>
                <mrow class="MJX-TeXAtom-ORD">
                  <mo>/</mo>
                </mrow>
                <mi>d</mi>
              </mrow>
            </mfrac>
          </msqrt>
        </mrow>
        <mo>=</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mfrac>
            <msqrt>
              <mi>n</mi>
              <msup>
                <mn>10</mn>
                <mrow class="MJX-TeXAtom-ORD">
                  <mn>2</mn>
                  <mi>N</mi>
                </mrow>
              </msup>
              <mrow class="MJX-TeXAtom-ORD">
                <mo>/</mo>
              </mrow>
              <mi>d</mi>
            </msqrt>
            <msup>
              <mn>10</mn>
              <mrow class="MJX-TeXAtom-ORD">
                <mi>N</mi>
              </mrow>
            </msup>
          </mfrac>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle {\sqrt {\frac {n}{d}}}={\sqrt {\frac {n10^{2N}/d}{d10^{2N}/d}}}={\frac {\sqrt {n10^{2N}/d}}{10^{N}}}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/7357ea34ff0bc90ad64ab9c0e2e86be310805674" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -2.838ex; width:34.424ex; height:8.676ex;" alt="{\displaystyle \sqrt{\frac{n}{d}} = \sqrt{&#10;\frac{n 10^{2N} / d}{d 10^{2N} / d}&#10;} = \frac{\sqrt{n 10^{2N} / d}}{10^N}}"></span>



so that what we need is one square root of a big number that we'll truncate to its integer part.  We'll use the method described in [Integer roots](https://rosettacode.org/wiki/Integer_roots) to compute the square root of this big integer.



Notice that we don't get the exact number of decimals required&#160;: the last two decimals or so can be wrong.  This is because we don't need <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle a\_{n}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msub>
          <mi>a</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </msub>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle a\_{n}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/790f9209748c2dca7ed7b81932c37c02af1dbc31" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.671ex; width:2.448ex; height:2.009ex;" alt="{\displaystyle a\_n}"></span>, but rather <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle a\_{n}^{2}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msubsup>
          <mi>a</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>2</mn>
          </mrow>
        </msubsup>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle a\_{n}^{2}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/b0155b9092f8c6f65539700013525837262e1d37" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.671ex; width:2.448ex; height:2.843ex;" alt="{\displaystyle a\_n^2}"></span>.  Elevating to the square makes us lose a bit of precision.  It could be compensated by choosing a slightly higher value of N (in a way that could be precisely calculated), but that would probably be overkill.

```perl
constant number-of-decimals = 100;

multi sqrt(Int $n) {
  (10**($n.chars div 2), { ($_ + $n div $_) div 2 } ... * == *).tail
}

multi sqrt(FatRat $r --> FatRat) {
  return FatRat.new:
    sqrt($r.numerator * 10**(number-of-decimals*2) div $r.denominator),
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
