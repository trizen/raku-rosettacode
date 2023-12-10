[1]: https://rosettacode.org/wiki/Continued_fraction

# [Continued fraction][1]



```perl
sub continued-fraction(:@a, :@b, Int :$n = 100)
{
    my $x = @a[$n - 1];
    $x = @a[$_ - 1] + @b[$_] / $x for reverse 1 ..^ $n;
    $x;
}

printf "√2 ≈%.9f\n", continued-fraction(:a(1, |(2 xx *)), :b(Nil, |(1 xx *)));
printf "e  ≈%.9f\n", continued-fraction(:a(2, |(1 .. *)), :b(Nil, 1, |(1 .. *)));
printf "π  ≈%.9f\n", continued-fraction(:a(3, |(6 xx *)), :b(Nil, |((1, 3, 5 ... *) X** 2)));
```

#### Output:
```
√2 ≈ 1.414213562
e  ≈ 2.718281828
π  ≈ 3.141592654
```


A more original and a bit more abstract method would consist in viewing a continued fraction on rank n as a function of a variable x:



Or, more consistently:



Viewed as such, <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \mathrm {CF} \_{n}(x)}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msub>
          <mrow class="MJX-TeXAtom-ORD">
            <mi mathvariant="normal">C</mi>
            <mi mathvariant="normal">F</mi>
          </mrow>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </msub>
        <mo stretchy="false">(</mo>
        <mi>x</mi>
        <mo stretchy="false">)</mo>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \mathrm {CF} \_{n}(x)}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/75b92bbc6f0ca4bee201a8dad88fa1b81da830f3" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.838ex; width:7.553ex; height:2.843ex;" alt="{\displaystyle \mathrm{CF}\_n(x)}"></span> could be written recursively:



Or in other words:



where <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle f\_{n}(x)=a\_{n}+{\frac {b\_{n}}{x}}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <msub>
          <mi>f</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </msub>
        <mo stretchy="false">(</mo>
        <mi>x</mi>
        <mo stretchy="false">)</mo>
        <mo>=</mo>
        <msub>
          <mi>a</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mi>n</mi>
          </mrow>
        </msub>
        <mo>+</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mfrac>
            <msub>
              <mi>b</mi>
              <mrow class="MJX-TeXAtom-ORD">
                <mi>n</mi>
              </mrow>
            </msub>
            <mi>x</mi>
          </mfrac>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle f\_{n}(x)=a\_{n}+{\frac {b\_{n}}{x}}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/197eec6df33679c436d579fa0a4a165b45bfb6fe" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -1.838ex; width:16.936ex; height:5.343ex;" alt="{\displaystyle f\_n(x) = a\_n + \frac{b\_n}{x}}"></span>



Raku has a builtin composition operator.  We can use it with the triangular reduction metaoperator, and evaluate each resulting function at infinity (any value would do actually, but infinite makes it consistent with this particular task).

```perl
sub continued-fraction(@a, @b) {
    map { .(Inf) }, [\o] map { @a[$_] + @b[$_] / * }, ^Inf
}
 
printf "√2 ≈ %.9f\n", continued-fraction((1, |(2 xx *)), (1 xx *))[10];
printf "e  ≈ %.9f\n", continued-fraction((2, |(1 .. *)), (1, |(1 .. *)))[10];
printf "π  ≈ %.9f\n", continued-fraction((3, |(6 xx *)), ((1, 3, 5 ... *) X** 2))[100];
```

#### Output:
```
√2 ≈ 1.414213552
e  ≈ 2.718281827
π  ≈ 3.141592411
```
