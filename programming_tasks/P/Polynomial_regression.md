[1]: https://rosettacode.org/wiki/Polynomial_regression

# [Polynomial regression][1]


We'll use a Clifford algebra library.  Very slow.



Rationale (in French for some reason):



Le système d'équations peut s'écrire&#160;:
<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \left(a+bx\_{i}+cx\_{i}^{2}=y\_{i}\right)\_{i=1\ldots N}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msub>
          <mrow>
            <mo>(</mo>
            <mrow>
              <mi>a</mi>
              <mo>+</mo>
              <mi>b</mi>
              <msub>
                <mi>x</mi>
                <mrow class="MJX-TeXAtom-ORD">
                  <mi>i</mi>
                </mrow>
              </msub>
              <mo>+</mo>
              <mi>c</mi>
              <msubsup>
                <mi>x</mi>
                <mrow class="MJX-TeXAtom-ORD">
                  <mi>i</mi>
                </mrow>
                <mrow class="MJX-TeXAtom-ORD">
                  <mn>2</mn>
                </mrow>
              </msubsup>
              <mo>=</mo>
              <msub>
                <mi>y</mi>
                <mrow class="MJX-TeXAtom-ORD">
                  <mi>i</mi>
                </mrow>
              </msub>
            </mrow>
            <mo>)</mo>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
            <mo>&#x2026;<!-- … --></mo>
            <mi>N</mi>
          </mrow>
        </msub>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \left(a+bx\_{i}+cx\_{i}^{2}=y\_{i}\right)\_{i=1\ldots N}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/c1688a0fcc47d943f106ff5ea42c7ef0248ceef4" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -1.171ex; width:26.88ex; height:3.343ex;" alt="{\displaystyle \left(a + b x\_i + cx\_i^2 = y\_i\right)\_{i=1\ldots N}}"></span>, où on cherche <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle (a,b,c)\in \mathbb {R} ^{3}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mo stretchy="false">(</mo>
        <mi>a</mi>
        <mo>,</mo>
        <mi>b</mi>
        <mo>,</mo>
        <mi>c</mi>
        <mo stretchy="false">)</mo>
        <mo>&#x2208;<!-- ∈ --></mo>
        <msup>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="double-struck">R</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>3</mn>
          </mrow>
        </msup>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle (a,b,c)\in \mathbb {R} ^{3}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/bc4a300857c498912158f1c79df5a5340b09f6b3" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:12.684ex; height:3.176ex;" alt="{\displaystyle (a,b,c)\in\mathbb{R}^3}"></span>.  On considère <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \mathbb {R} ^{N}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msup>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="double-struck">R</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>N</mi>
          </mrow>
        </msup>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \mathbb {R} ^{N}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/12d5be9beb2f7a56cdee3c6563c9453a913a0c92" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:3.37ex; height:2.676ex;" alt="{\displaystyle \mathbb{R}^N}"></span> et on répartit chaque équation sur chaque dimension:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle (a+bx\_{i}+cx\_{i}^{2})\mathbf {e} \_{i}=y\_{i}\mathbf {e} \_{i}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mo stretchy="false">(</mo>
        <mi>a</mi>
        <mo>+</mo>
        <mi>b</mi>
        <msub>
          <mi>x</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <mo>+</mo>
        <mi>c</mi>
        <msubsup>
          <mi>x</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>2</mn>
          </mrow>
        </msubsup>
        <mo stretchy="false">)</mo>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">e</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <mo>=</mo>
        <msub>
          <mi>y</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">e</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle (a+bx\_{i}+cx\_{i}^{2})\mathbf {e} \_{i}=y\_{i}\mathbf {e} \_{i}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/d6683bb816a07dca5cab6dd44a1ac5bb4038fa87" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -1.005ex; width:24.325ex; height:3.176ex;" alt="{\displaystyle  (a + b x\_i + cx\_i^2)\mathbf{e}\_i = y\_i\mathbf{e}\_i}"></span>



Posons alors&#160;:



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \mathbf {x} \_{0}=\sum \_{i=1}^{N}\mathbf {e} \_{i},\,\mathbf {x} \_{1}=\sum \_{i=1}^{N}x\_{i}\mathbf {e} \_{i},\,\mathbf {x} \_{2}=\sum \_{i=1}^{N}x\_{i}^{2}\mathbf {e} \_{i},\,\mathbf {y} =\sum \_{i=1}^{N}y\_{i}\mathbf {e} \_{i}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">x</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>0</mn>
          </mrow>
        </msub>
        <mo>=</mo>
        <munderover>
          <mo>&#x2211;<!-- ∑ --></mo>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>N</mi>
          </mrow>
        </munderover>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">e</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <mo>,</mo>
        <mspace width="thinmathspace" />
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">x</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>1</mn>
          </mrow>
        </msub>
        <mo>=</mo>
        <munderover>
          <mo>&#x2211;<!-- ∑ --></mo>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>N</mi>
          </mrow>
        </munderover>
        <msub>
          <mi>x</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">e</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <mo>,</mo>
        <mspace width="thinmathspace" />
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">x</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>2</mn>
          </mrow>
        </msub>
        <mo>=</mo>
        <munderover>
          <mo>&#x2211;<!-- ∑ --></mo>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>N</mi>
          </mrow>
        </munderover>
        <msubsup>
          <mi>x</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>2</mn>
          </mrow>
        </msubsup>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">e</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <mo>,</mo>
        <mspace width="thinmathspace" />
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">y</mi>
        </mrow>
        <mo>=</mo>
        <munderover>
          <mo>&#x2211;<!-- ∑ --></mo>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
            <mo>=</mo>
            <mn>1</mn>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>N</mi>
          </mrow>
        </munderover>
        <msub>
          <mi>y</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">e</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>i</mi>
          </mrow>
        </msub>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \mathbf {x} \_{0}=\sum \_{i=1}^{N}\mathbf {e} \_{i},\,\mathbf {x} \_{1}=\sum \_{i=1}^{N}x\_{i}\mathbf {e} \_{i},\,\mathbf {x} \_{2}=\sum \_{i=1}^{N}x\_{i}^{2}\mathbf {e} \_{i},\,\mathbf {y} =\sum \_{i=1}^{N}y\_{i}\mathbf {e} \_{i}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/173f303bd25655d9f69ff63822b5a8b69301919e" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -3.005ex; width:54.983ex; height:7.343ex;" alt="{\displaystyle &#10;\mathbf{x}\_0 = \sum\_{i=1}^N \mathbf{e}\_i,\,&#10;\mathbf{x}\_1 = \sum\_{i=1}^N x\_i\mathbf{e}\_i,\,&#10;\mathbf{x}\_2 = \sum\_{i=1}^N x\_i^2\mathbf{e}\_i,\,&#10;\mathbf{y}   = \sum\_{i=1}^N y\_i\mathbf{e}\_i&#10;}"></span>



Le système d'équations devient&#160;: <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle a\mathbf {x} \_{0}+b\mathbf {x} \_{1}+c\mathbf {x} \_{2}=\mathbf {y} }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>a</mi>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">x</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>0</mn>
          </mrow>
        </msub>
        <mo>+</mo>
        <mi>b</mi>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">x</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>1</mn>
          </mrow>
        </msub>
        <mo>+</mo>
        <mi>c</mi>
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="bold">x</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>2</mn>
          </mrow>
        </msub>
        <mo>=</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="bold">y</mi>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle a\mathbf {x} \_{0}+b\mathbf {x} \_{1}+c\mathbf {x} \_{2}=\mathbf {y} }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/e056ef42343053cb3f937006e1d47d46ccc38dcc" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.671ex; width:20.82ex; height:2.509ex;" alt="{\displaystyle a\mathbf{x}\_0+b\mathbf{x}\_1+c\mathbf{x}\_2 = \mathbf{y}}"></span>.



D'où&#160;:
<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle {\begin{aligned}a=\mathbf {y} \land \mathbf {x} \_{1}\land \mathbf {x} \_{2}/(\mathbf {x} \_{0}\land \mathbf {x\_{1}} \land \mathbf {x\_{2}} )\\b=\mathbf {y} \land \mathbf {x} \_{2}\land \mathbf {x} \_{0}/(\mathbf {x} \_{1}\land \mathbf {x\_{2}} \land \mathbf {x\_{0}} )\\c=\mathbf {y} \land \mathbf {x} \_{0}\land \mathbf {x} \_{1}/(\mathbf {x} \_{2}\land \mathbf {x\_{0}} \land \mathbf {x\_{1}} )\\\end{aligned}}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mrow class="MJX-TeXAtom-ORD">
          <mtable columnalign="right left right left right left right left right left right left" rowspacing="3pt" columnspacing="0em 2em 0em 2em 0em 2em 0em 2em 0em 2em 0em" displaystyle="true">
            <mtr>
              <mtd>
                <mi>a</mi>
                <mo>=</mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <mi mathvariant="bold">y</mi>
                </mrow>
                <mo>&#x2227;<!-- ∧ --></mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>1</mn>
                  </mrow>
                </msub>
                <mo>&#x2227;<!-- ∧ --></mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                  </mrow>
                </msub>
                <mrow class="MJX-TeXAtom-ORD">
                  <mo>/</mo>
                </mrow>
                <mo stretchy="false">(</mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>0</mn>
                  </mrow>
                </msub>
                <mo>&#x2227;<!-- ∧ --></mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">x</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">1</mn>
                    </mrow>
                  </msub>
                </mrow>
                <mo>&#x2227;<!-- ∧ --></mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">x</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">2</mn>
                    </mrow>
                  </msub>
                </mrow>
                <mo stretchy="false">)</mo>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mi>b</mi>
                <mo>=</mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <mi mathvariant="bold">y</mi>
                </mrow>
                <mo>&#x2227;<!-- ∧ --></mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                  </mrow>
                </msub>
                <mo>&#x2227;<!-- ∧ --></mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>0</mn>
                  </mrow>
                </msub>
                <mrow class="MJX-TeXAtom-ORD">
                  <mo>/</mo>
                </mrow>
                <mo stretchy="false">(</mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>1</mn>
                  </mrow>
                </msub>
                <mo>&#x2227;<!-- ∧ --></mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">x</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">2</mn>
                    </mrow>
                  </msub>
                </mrow>
                <mo>&#x2227;<!-- ∧ --></mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">x</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">0</mn>
                    </mrow>
                  </msub>
                </mrow>
                <mo stretchy="false">)</mo>
              </mtd>
            </mtr>
            <mtr>
              <mtd>
                <mi>c</mi>
                <mo>=</mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <mi mathvariant="bold">y</mi>
                </mrow>
                <mo>&#x2227;<!-- ∧ --></mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>0</mn>
                  </mrow>
                </msub>
                <mo>&#x2227;<!-- ∧ --></mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>1</mn>
                  </mrow>
                </msub>
                <mrow class="MJX-TeXAtom-ORD">
                  <mo>/</mo>
                </mrow>
                <mo stretchy="false">(</mo>
                <msub>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mi mathvariant="bold">x</mi>
                  </mrow>
                  <mrow class="MJX-TeXAtom-ORD">
                    <mn>2</mn>
                  </mrow>
                </msub>
                <mo>&#x2227;<!-- ∧ --></mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">x</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">0</mn>
                    </mrow>
                  </msub>
                </mrow>
                <mo>&#x2227;<!-- ∧ --></mo>
                <mrow class="MJX-TeXAtom-ORD">
                  <msub>
                    <mi mathvariant="bold">x</mi>
                    <mrow class="MJX-TeXAtom-ORD">
                      <mn mathvariant="bold">1</mn>
                    </mrow>
                  </msub>
                </mrow>
                <mo stretchy="false">)</mo>
              </mtd>
            </mtr>
          </mtable>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle {\begin{aligned}a=\mathbf {y} \land \mathbf {x} \_{1}\land \mathbf {x} \_{2}/(\mathbf {x} \_{0}\land \mathbf {x\_{1}} \land \mathbf {x\_{2}} )\\b=\mathbf {y} \land \mathbf {x} \_{2}\land \mathbf {x} \_{0}/(\mathbf {x} \_{1}\land \mathbf {x\_{2}} \land \mathbf {x\_{0}} )\\c=\mathbf {y} \land \mathbf {x} \_{0}\land \mathbf {x} \_{1}/(\mathbf {x} \_{2}\land \mathbf {x\_{0}} \land \mathbf {x\_{1}} )\\\end{aligned}}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/06e31fa1c15f3c72b5992789b54108f3184bf150" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -4.005ex; width:32.365ex; height:9.176ex;" alt="{\displaystyle \begin{align}&#10;a = \mathbf{y}\and\mathbf{x}\_1\and\mathbf{x}\_2/(\mathbf{x}\_0\and\mathbf{x\_1}\and\mathbf{x\_2})\\&#10;b = \mathbf{y}\and\mathbf{x}\_2\and\mathbf{x}\_0/(\mathbf{x}\_1\and\mathbf{x\_2}\and\mathbf{x\_0})\\&#10;c = \mathbf{y}\and\mathbf{x}\_0\and\mathbf{x}\_1/(\mathbf{x}\_2\and\mathbf{x\_0}\and\mathbf{x\_1})\\&#10;\end{align}}"></span>

```perl
use MultiVector;

constant @x1 = <0 1 2 3 4 5 6 7 8 9 10>;
constant @y = <1 6 17 34 57 86 121 162 209 262 321>;

constant $x0 = [+] @e[^@x1];
constant $x1 = [+] @x1 Z* @e;
constant $x2 = [+] @x1 »**» 2  Z* @e;

constant $y  = [+] @y Z* @e;

.say for
  $y∧$x1∧$x2/($x0∧$x1∧$x2),
  $y∧$x2∧$x0/($x1∧$x2∧$x0),
  $y∧$x0∧$x1/($x2∧$x0∧$x1);
```

#### Output:
```
1
2
3
```
