[1]: https://rosettacode.org/wiki/Monte_Carlo_methods

# [Monte Carlo methods][1]





We'll consider the upper-right quarter of the unitary disk centered at the origin.  Its area is <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle \pi  \over 4}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mfrac>
        <mstyle displaystyle="true" scriptlevel="0">
          <mi>&#x03C0;<!-- π --></mi>
        </mstyle>
        <mn>4</mn>
      </mfrac>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle \pi  \over 4}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/a6a0b09884a506293568263efe49ac399804925b" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -1.838ex; width:2.168ex; height:4.676ex;" alt="{\displaystyle \pi \over 4}"></span>.

```perl
my @random_distances = ([+] rand**2 xx 2) xx *;

sub approximate_pi(Int $n) {
    4 * @random_distances[^$n].grep(* < 1) / $n
}

say "Monte-Carlo π approximation:";
say "$_ iterations:  ", approximate_pi $_
    for 100, 1_000, 10_000;
```

#### Output:
```
Monte-Carlo π approximation:
100 iterations:  2.88
1000 iterations:  3.096
10000 iterations:  3.1168
```


We don't really need to write a function, though.  A lazy list would do:

```perl
my @pi = ([\+] 4 * (1 > [+] rand**2 xx 2) xx *) Z/ 1 .. *;
say @pi[10, 1000, 10_000];
```
