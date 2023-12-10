[1]: https://rosettacode.org/wiki/Polynomial_long_division

# [Polynomial long division][1]



```perl
sub poly_long_div ( @n is copy, @d ) {
    return [0], |@n if +@n < +@d;

    my @q = gather while +@n >= +@d {
        @n = @n Z- flat ( ( @d X* take ( @n[0] / @d[0] ) ), 0 xx * );
        @n.shift;
    }

    return @q, @n;
}

sub xP ( $power ) { $power>1 ?? "x^$power" !! $power==1 ?? 'x' !! '' }
sub poly_print ( @c ) { join ' + ', @c.kv.map: { $^v ~ xP( @c.end - $^k ) } }

my @polys = [ [     1, -12, 0, -42 ], [    1, -3 ] ],
            [ [     1, -12, 0, -42 ], [ 1, 1, -3 ] ],
            [ [          1, 3,   2 ], [    1,  1 ] ],
            [ [ 1, -4,   6, 5,   3 ], [ 1, 2,  1 ] ];

say '<math>\begin{array}{rr}';
for @polys -> [ @a, @b ] {
    printf Q"%s , & %s \\\\\n", poly_long_div( @a, @b ).map: { poly_print($_) };
}
say '\end{array}</math>';
```


Output:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle {\begin{array}{rr}1x^{2}+-9x+-27,&amp;-123\\1x+-13,&amp;16x+-81\\1x+2,&amp;0\\1x^{2}+-6x+17,&amp;-23x+-14\\\end{array}}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mrow class="MJX-TeXAtom-ORD">
          <mtable columnalign="right right" rowspacing="4pt" columnspacing="1em">
            <mtr>
              <mtd>
                <mn>1</mn>
                <msup>
                  <mi>x</mi>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                  </mrow>
                </msup>
                <mo>+</mo>
                <mo>&#x2212;<!-- − --></mo>
                <mn>9</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mo>&#x2212;<!-- − --></mo>
                <mn>27</mn>
                <mo>,</mo>
              </mtd>
              <mtd>
                <mo>&#x2212;<!-- − --></mo>
                <mn>123</mn>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mn>1</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mo>&#x2212;<!-- − --></mo>
                <mn>13</mn>
                <mo>,</mo>
              </mtd>
              <mtd>
                <mn>16</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mo>&#x2212;<!-- − --></mo>
                <mn>81</mn>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mn>1</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mn>2</mn>
                <mo>,</mo>
              </mtd>
              <mtd>
                <mn>0</mn>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mn>1</mn>
                <msup>
                  <mi>x</mi>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                  </mrow>
                </msup>
                <mo>+</mo>
                <mo>&#x2212;<!-- − --></mo>
                <mn>6</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mn>17</mn>
                <mo>,</mo>
              </mtd>
              <mtd>
                <mo>&#x2212;<!-- − --></mo>
                <mn>23</mn>
                <mi>x</mi>
                <mo>+</mo>
                <mo>&#x2212;<!-- − --></mo>
                <mn>14</mn>
              </mtd>
            </mtr>
          </mtable>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle {\begin{array}{rr}1x^{2}+-9x+-27,&amp;-123\\1x+-13,&amp;16x+-81\\1x+2,&amp;0\\1x^{2}+-6x+17,&amp;-23x+-14\\\end{array}}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/62f3feee0b614ae49e1563c08438ef1c0f80831a" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -5.838ex; width:33.818ex; height:12.843ex;" alt="{\displaystyle \begin{array}{rr}&#10;1x^2 + -9x + -27 , &amp; -123 \\&#10;1x + -13 , &amp; 16x + -81 \\&#10;1x + 2 , &amp; 0 \\&#10;1x^2 + -6x + 17 , &amp; -23x + -14 \\&#10;\end{array}}"></span>
