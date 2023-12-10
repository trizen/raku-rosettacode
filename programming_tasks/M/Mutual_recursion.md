[1]: https://rosettacode.org/wiki/Mutual_recursion

# [Mutual recursion][1]


A direct translation of the definitions of <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle F}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>F</mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle F}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/545fd099af8541605f7ee55f08225526be88ce57" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:1.741ex; height:2.176ex;" alt="{\displaystyle F}"></span> and <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle M}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>M</mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle M}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/f82cade9898ced02fdd08712e5f0c0151758a0dd" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:2.442ex; height:2.176ex;" alt="{\displaystyle M}"></span>:

```perl
multi F(0) { 1 }; multi M(0) { 0 }
multi F(\𝑛) { 𝑛 - M(F(𝑛 - 1)) }
multi M(\𝑛) { 𝑛 - F(M(𝑛 - 1)) }

say map &F, ^20;
say map &M, ^20;
```

#### Output:
```
1 1 2 2 3 3 4 5 5 6 6 7 8 8 9 9 10 11 11 12
0 0 1 2 2 3 4 4 5 6 6 7 7 8 9 9 10 11 11 12
```
