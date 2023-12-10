[1]: https://rosettacode.org/wiki/Combinations_and_permutations

# [Combinations and permutations][1]


Raku can't compute arbitrary large floating point values, thus we will use logarithms, as is often needed when dealing with combinations.   We'll also use a Stirling method to approximate <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \ln(n!)}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>ln</mi>
        <mo>&#x2061;<!-- ⁡ --></mo>
        <mo stretchy="false">(</mo>
        <mi>n</mi>
        <mo>!</mo>
        <mo stretchy="false">)</mo>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \ln(n!)}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/d264f09c80b779e9028658543d5e31ae647cacf1" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:5.79ex; height:2.843ex;" alt="{\displaystyle \ln(n!)}"></span>:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \ln n!\approx {\frac {1}{2}}\ln(2\pi n)+n\ln \left({\frac {n}{e}}+{\frac {1}{12en}}\right)}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>ln</mi>
        <mo>&#x2061;<!-- ⁡ --></mo>
        <mi>n</mi>
        <mo>!</mo>
        <mo>&#x2248;<!-- ≈ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mfrac>
            <mn>1</mn>
            <mn>2</mn>
          </mfrac>
        </mrow>
        <mi>ln</mi>
        <mo>&#x2061;<!-- ⁡ --></mo>
        <mo stretchy="false">(</mo>
        <mn>2</mn>
        <mi>&#x03C0;<!-- π --></mi>
        <mi>n</mi>
        <mo stretchy="false">)</mo>
        <mo>+</mo>
        <mi>n</mi>
        <mi>ln</mi>
        <mo>&#x2061;<!-- ⁡ --></mo>
        <mrow>
          <mo>(</mo>
          <mrow>
            <mrow class="MJX-TeXAtom-ORD">
              <mfrac>
                <mi>n</mi>
                <mi>e</mi>
              </mfrac>
            </mrow>
            <mo>+</mo>
            <mrow class="MJX-TeXAtom-ORD">
              <mfrac>
                <mn>1</mn>
                <mrow>
                  <mn>12</mn>
                  <mi>e</mi>
                  <mi>n</mi>
                </mrow>
              </mfrac>
            </mrow>
          </mrow>
          <mo>)</mo>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \ln n!\approx {\frac {1}{2}}\ln(2\pi n)+n\ln \left({\frac {n}{e}}+{\frac {1}{12en}}\right)}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/c30535e750163de96d5626c9d81642fd66739d34" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -2.505ex; width:38.183ex; height:6.176ex;" alt="{\displaystyle \ln n! \approx&#10;\frac{1}{2}\ln(2\pi n) + n\ln\left(\frac{n}{e} + \frac{1}{12 e n}\right)}"></span>



Notice that Raku can process arbitrary long integers, though.  So it's not clear whether using floating points is useful in this case.

```perl
multi P($n, $k) { [*] $n - $k + 1 .. $n }
multi C($n, $k) { P($n, $k) / [*] 1 .. $k }
 
sub lstirling(\n) {
    n < 10 ?? lstirling(n+1) - log(n+1) !!
    .5*log(2*pi*n)+ n*log(n/e+1/(12*e*n))
}
 
role Logarithm {
    method gist {
	my $e = (self/10.log).Int;
	sprintf "%.8fE%+d", exp(self - $e*10.log), $e;
    }
}
multi P($n, $k, :$float!) {
    (lstirling($n) - lstirling($n -$k))
    but Logarithm
}
multi C($n, $k, :$float!) {
    (lstirling($n) - lstirling($n -$k) - lstirling($k))
    but Logarithm
}
 
say "Exact results:";
for 1..12 -> $n {
    my $p = $n div 3;
    say "P($n, $p) = ", P($n, $p);
}
 
for 10, 20 ... 60 -> $n {
    my $p = $n div 3;
    say "C($n, $p) = ", C($n, $p);
}
 
say '';
say "Floating point approximations:";
for 5, 50, 500, 1000, 5000, 15000 -> $n {
    my $p = $n div 3;
    say "P($n, $p) = ", P($n, $p, :float);
}
 
for 100, 200 ... 1000 -> $n {
    my $p = $n div 3;
    say "C($n, $p) = ", C($n, $p, :float);
}
```

#### Output:
```
Exact results:
P(1, 0) = 1
P(2, 0) = 1
P(3, 1) = 3
P(4, 1) = 4
P(5, 1) = 5
P(6, 2) = 30
P(7, 2) = 42
P(8, 2) = 56
P(9, 3) = 504
P(10, 3) = 720
P(11, 3) = 990
P(12, 4) = 11880
C(10, 3) = 120
C(20, 6) = 38760
C(30, 10) = 30045015
C(40, 13) = 12033222880
C(50, 16) = 4923689695575
C(60, 20) = 4191844505805495

Floating point approximations:
P(5, 1) = 5.00000000E+0
P(50, 16) = 1.03017326E+26
P(500, 166) = 3.53487492E+434
P(1000, 333) = 5.96932629E+971
P(5000, 1666) = 6.85674576E+6025
P(15000, 5000) = 9.64985399E+20469
C(100, 33) = 2.94692433E+26
C(200, 66) = 7.26975256E+53
C(300, 100) = 4.15825147E+81
C(400, 133) = 1.25794868E+109
C(500, 166) = 3.92602839E+136
C(600, 200) = 2.50601778E+164
C(700, 233) = 8.10320356E+191
C(800, 266) = 2.64562336E+219
C(900, 300) = 1.74335637E+247
C(1000, 333) = 5.77613455E+274
```
