[1]: https://rosettacode.org/wiki/Permutations_with_repetitions

# [Permutations with repetitions][1]





We can use the `X` operator ("cartesian product") to cross the list with itself.

For <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle n=2}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>n</mi>
        <mo>=</mo>
        <mn>2</mn>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle n=2}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/a02c8bd752d2cc859747ca1f3a508281bdbc3b34" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:5.656ex; height:2.176ex;" alt="{\displaystyle n=2}"></span>:

```perl
my @k = <a b c>;

.say for @k X @k;
```


For arbitrary <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle n}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>n</mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle n}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/a601995d55609f2d9f5e233e36fbe9ea26011b3b" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:1.395ex; height:1.676ex;" alt="{\displaystyle n}"></span>:

```perl
my @k = <a b c>;
my $n = 2;

.say for [X] @k xx $n;
```

#### Output:
```
a a
a b
a c
b a
b b
b c
c a
c b
c c
```


Here is an other approach, counting all <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle k^{n}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msup>
          <mi>k</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </msup>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle k^{n}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/29d0ca5fd176db2867ec07a961a31f17bc6fb07e" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:2.43ex; height:2.343ex;" alt="{\displaystyle k^n}"></span> possibilities in base <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle k}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>k</mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle k}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/c3c9a2c7b599b37105512c5d570edc034056dd40" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:1.211ex; height:2.176ex;" alt="{\displaystyle k}"></span>:

```perl
my @k = <a b c>;
my $n = 2;

say @k[.polymod: +@k xx $n-1] for ^@k**$n
```

#### Output:
```
a a
b a
c a
a b
b b
c b
a c
b c
c c
```
