[1]: https://rosettacode.org/wiki/Multiple_regression

# [Multiple regression][1]


We're going to solve the example on the Wikipedia article using [Clifford](https://github.com/grondilu/clifford), a [geometric algebra](https://en.wikipedia.org/wiki/Geometric_algebra) module.  Optimization for large vector space does not quite work yet, so it's going to take (a lof of) time and a fair amount of memory, but it should work.



Let's create four vectors containing our input data:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle {\begin{aligned}\mathbf {w} &amp;=w^{k}\mathbf {e} \_{k}\\\mathbf {h\_{0}} &amp;=(h^{k})^{0}\mathbf {e} \_{k}\\\mathbf {h\_{1}} &amp;=(h^{k})^{1}\mathbf {e} \_{k}\\\mathbf {h\_{2}} &amp;=(h^{k})^{2}\mathbf {e} \_{k}\end{aligned}}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mrow class="MJX-TeXAtom-ORD">
          <mtable columnalign="right left right left right left right left right left right left" rowspacing="3pt" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" displaystyle="true">
            <mtr>
              <mtd>
                <mrow class="MJX-TeXAtom-ORD">
                  <mi mathvariant="bold">w</mi>
                </mrow>
              </mtd>
              <mtd>
                <mi></mi>
                <mo>=</mo>
                <msup>
                  <mi>w</mi>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msup>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">e</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msub>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">h</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">0</mn>
                    </mrow>
                  </msub>
                </mrow>
              </mtd>
              <mtd>
                <mi></mi>
                <mo>=</mo>
                <mo stretchy="false">(</mo>
                <msup>
                  <mi>h</mi>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msup>
                <msup>
                  <mo stretchy="false">)</mo>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>0</mn>
                  </mrow>
                </msup>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">e</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msub>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">h</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">1</mn>
                    </mrow>
                  </msub>
                </mrow>
              </mtd>
              <mtd>
                <mi></mi>
                <mo>=</mo>
                <mo stretchy="false">(</mo>
                <msup>
                  <mi>h</mi>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msup>
                <msup>
                  <mo stretchy="false">)</mo>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>1</mn>
                  </mrow>
                </msup>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">e</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msub>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">h</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">2</mn>
                    </mrow>
                  </msub>
                </mrow>
              </mtd>
              <mtd>
                <mi></mi>
                <mo>=</mo>
                <mo stretchy="false">(</mo>
                <msup>
                  <mi>h</mi>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msup>
                <msup>
                  <mo stretchy="false">)</mo>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                  </mrow>
                </msup>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">e</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi>k</mi>
                  </mrow>
                </msub>
              </mtd>
            </mtr>
          </mtable>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle {\begin{aligned}\mathbf {w} &amp;=w^{k}\mathbf {e} \_{k}\\\mathbf {h\_{0}} &amp;=(h^{k})^{0}\mathbf {e} \_{k}\\\mathbf {h\_{1}} &amp;=(h^{k})^{1}\mathbf {e} \_{k}\\\mathbf {h\_{2}} &amp;=(h^{k})^{2}\mathbf {e} \_{k}\end{aligned}}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/bc15f9968574838b0ad942d023540c1051a6d409" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -6.005ex; width:14.118ex; height:13.176ex;" alt="{\displaystyle \begin{align}&#10;\mathbf{w} &amp; = w^k\mathbf{e}\_k\\&#10;\mathbf{h\_0} &amp; = (h^k)^0\mathbf{e}\_k\\&#10;\mathbf{h\_1} &amp; = (h^k)^1\mathbf{e}\_k\\&#10;\mathbf{h\_2} &amp; = (h^k)^2\mathbf{e}\_k&#10;\end{align}}"></span>



Then what we're looking for are three scalars <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \alpha }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B1;<!-- α --></mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \alpha }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/b79333175c8b3f0840bfb4ec41b8072c83ea88d3" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:1.488ex; height:1.676ex;" alt="{\displaystyle \alpha}"></span>, <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \beta }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B2;<!-- β --></mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \beta }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/7ed48a5e36207156fb792fa79d29925d2f7901e8" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.671ex; width:1.332ex; height:2.509ex;" alt="{\displaystyle \beta}"></span> and <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \gamma }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B3;<!-- γ --></mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \gamma }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/a223c880b0ce3da8f64ee33c4f0010beee400b1a" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:1.262ex; height:2.176ex;" alt="{\displaystyle \gamma}"></span> such that:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \alpha \mathbf {h0} +\beta \mathbf {h1} +\gamma \mathbf {h2} =\mathbf {w} }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B1;<!-- α --></mi>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">0</mn>
        </mrow>
        <mo>+</mo>
        <mi>&#x03B2;<!-- β --></mi>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">1</mn>
        </mrow>
        <mo>+</mo>
        <mi>&#x03B3;<!-- γ --></mi>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">2</mn>
        </mrow>
        <mo>=</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">w</mi>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \alpha \mathbf {h0} +\beta \mathbf {h1} +\gamma \mathbf {h2} =\mathbf {w} }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/2f3676477c1744d4074b9ff8cdf9fb6ad366c552" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:23.258ex; height:2.676ex;" alt="{\displaystyle \alpha\mathbf{h0} + \beta\mathbf{h1} + \gamma\mathbf{h2} = \mathbf{w}}"></span>



To get for instance <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \alpha }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B1;<!-- α --></mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \alpha }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/b79333175c8b3f0840bfb4ec41b8072c83ea88d3" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:1.488ex; height:1.676ex;" alt="{\displaystyle \alpha}"></span> we can first make the <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \beta }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B2;<!-- β --></mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \beta }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/7ed48a5e36207156fb792fa79d29925d2f7901e8" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.671ex; width:1.332ex; height:2.509ex;" alt="{\displaystyle \beta}"></span> and <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \gamma }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B3;<!-- γ --></mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \gamma }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/a223c880b0ce3da8f64ee33c4f0010beee400b1a" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:1.262ex; height:2.176ex;" alt="{\displaystyle \gamma}"></span> terms disappear:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \alpha \mathbf {h0} \wedge \mathbf {h1} \wedge \mathbf {h2} =\mathbf {w} \wedge \mathbf {h1} \wedge \mathbf {h2} }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B1;<!-- α --></mi>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">0</mn>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">1</mn>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">2</mn>
        </mrow>
        <mo>=</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">w</mi>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">1</mn>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">2</mn>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \alpha \mathbf {h0} \wedge \mathbf {h1} \wedge \mathbf {h2} =\mathbf {w} \wedge \mathbf {h1} \wedge \mathbf {h2} }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/9c3f67a998ae6125fe2389f04041be3f24ab1ea0" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:30.957ex; height:2.176ex;" alt="{\displaystyle \alpha\mathbf{h0}\wedge\mathbf{h1}\wedge\mathbf{h2} = \mathbf{w}\wedge\mathbf{h1}\wedge\mathbf{h2}}"></span>



Noting <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle I=\mathbf {h0} \wedge \mathbf {h1} \wedge \mathbf {h2} }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>I</mi>
        <mo>=</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">0</mn>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">1</mn>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">2</mn>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle I=\mathbf {h0} \wedge \mathbf {h1} \wedge \mathbf {h2} }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/f8fd7df5ac9febe02b9e957194c797a37f1b9260" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:17.901ex; height:2.176ex;" alt="{\displaystyle I = \mathbf{h0}\wedge\mathbf{h1}\wedge\mathbf{h2}}"></span>, we then get:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \alpha =(\mathbf {w} \wedge \mathbf {h1} \wedge \mathbf {h2} )\cdot {\tilde {I}}/I\cdot {\tilde {I}}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>&#x03B1;<!-- α --></mi>
        <mo>=</mo>
        <mo stretchy="false">(</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">w</mi>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">1</mn>
        </mrow>
        <mo>&#x2227;<!-- ∧ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">h</mi>
          <mn mathvariant="bold">2</mn>
        </mrow>
        <mo stretchy="false">)</mo>
        <mo>&#x22C5;<!-- ⋅ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mrow class="MJX-TeXAtom-ORD">
            <mover>
              <mi>I</mi>
              <mo stretchy="false">&#x007E;<!-- ~ --></mo>
            </mover>
          </mrow>
        </mrow>
        <mrow class="MJX-TeXAtom-ORD">
          <mo>/</mo>
        </mrow>
        <mi>I</mi>
        <mo>&#x22C5;<!-- ⋅ --></mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mrow class="MJX-TeXAtom-ORD">
            <mover>
              <mi>I</mi>
              <mo stretchy="false">&#x007E;<!-- ~ --></mo>
            </mover>
          </mrow>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \alpha =(\mathbf {w} \wedge \mathbf {h1} \wedge \mathbf {h2} )\cdot {\tilde {I}}/I\cdot {\tilde {I}}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/c20866d1bd2a547e5b0a2027e11be1f4cb42874a" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:27.871ex; height:3.176ex;" alt="{\displaystyle \alpha = (\mathbf{w}\wedge\mathbf{h1}\wedge\mathbf{h2})\cdot\tilde{I}/I\cdot\tilde{I}}"></span>



**Note:** a number of the formulae above are invisible to the majority of browsers, including Chrome, IE/Edge, Safari and Opera. They may (subject to the installation of necessary fronts) be visible to Firefox.






```perl
use Clifford;
my @height = <1.47 1.50 1.52 1.55 1.57 1.60 1.63 1.65 1.68 1.70 1.73 1.75 1.78 1.80 1.83>;
my @weight = <52.21 53.12 54.48 55.84 57.20 58.57 59.93 61.29 63.11 64.47 66.28 68.10 69.92 72.19 74.46>;

my $w = [+] @weight Z* @e;

my $h0 = [+] @e[^@weight];
my $h1 = [+] @height Z* @e;
my $h2 = [+] (@height X** 2) Z* @e;

my $I = $h0∧$h1∧$h2;
my $I2 = ($I·$I.reversion).Real;

say "α = ", ($w∧$h1∧$h2)·$I.reversion/$I2;
say "β = ", ($w∧$h2∧$h0)·$I.reversion/$I2;
say "γ = ", ($w∧$h0∧$h1)·$I.reversion/$I2;
```

#### Output:
```
α = 128.81280357844
β = -143.1620228648
γ = 61.960325442
```


April 2016: This computation took over an hour with MoarVM, running in a VirtualBox linux system guest hosted by a windows laptop with a i7 intel processor.

March 2020: With various improvements to the language, 6.d releases of Raku now run the code 2x to 3x as fast, depending on the hardware used, but even so it is generous to describe it as 'slow'.
