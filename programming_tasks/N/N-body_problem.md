[1]: https://rosettacode.org/wiki/N-body_problem

# [N-body problem][1]





We'll try to simulate the Sun+Earth+Moon system, with plausible astronomical values.



We use a 18-dimension vector <span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle ABC}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mi>A</mi>
        <mi>B</mi>
        <mi>C</mi>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle ABC}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/5e55b44cfd965fbdc7a328d5db8a35a619db0971" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -0.338ex; width:5.273ex; height:2.176ex;" alt="{\displaystyle ABC}"></span>.  The first nine dimensions are the positions of the three bodies.  The other nine are the velocities.  This allows us to write the dynamics as a first-temporal derivative equation, since



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle {\frac {d\mathrm {position} }{dt}}=\mathrm {speed} }">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mrow class="MJX-TeXAtom-ORD">
          <mfrac>
            <mrow>
              <mi>d</mi>
              <mrow class="MJX-TeXAtom-ORD">
                <mi mathvariant="normal">p</mi>
                <mi mathvariant="normal">o</mi>
                <mi mathvariant="normal">s</mi>
                <mi mathvariant="normal">i</mi>
                <mi mathvariant="normal">t</mi>
                <mi mathvariant="normal">i</mi>
                <mi mathvariant="normal">o</mi>
                <mi mathvariant="normal">n</mi>
              </mrow>
            </mrow>
            <mrow>
              <mi>d</mi>
              <mi>t</mi>
            </mrow>
          </mfrac>
        </mrow>
        <mo>=</mo>
        <mrow class="MJX-TeXAtom-ORD">
          <mi mathvariant="normal">s</mi>
          <mi mathvariant="normal">p</mi>
          <mi mathvariant="normal">e</mi>
          <mi mathvariant="normal">e</mi>
          <mi mathvariant="normal">d</mi>
        </mrow>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle {\frac {d\mathrm {position} }{dt}}=\mathrm {speed} }</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/c7795515d0ba3dc9f8b14c0998486e927946f1d0" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -2.005ex; width:18.741ex; height:5.509ex;" alt="{\displaystyle \frac{d\mathrm{position}}{dt} = \mathrm{speed}}"></span>



and thus



<span class="mwe-math-element"><span class="mwe-math-mathml-inline mwe-math-mathml-a11y" style="display: none;"><math xmlns="https://www.w3.org/1998/Math/MathML"  alttext="{\displaystyle {\frac {dABC\_{1..9}}{dt}}=ABC\_{10..18}}">
  <semantics>
    <mrow class="MJX-TeXAtom-ORD">
      <mstyle displaystyle="true" scriptlevel="0">
        <mrow class="MJX-TeXAtom-ORD">
          <mfrac>
            <mrow>
              <mi>d</mi>
              <mi>A</mi>
              <mi>B</mi>
              <msub>
                <mi>C</mi>
                <mrow class="MJX-TeXAtom-ORD">
                  <mn>1..9</mn>
                </mrow>
              </msub>
            </mrow>
            <mrow>
              <mi>d</mi>
              <mi>t</mi>
            </mrow>
          </mfrac>
        </mrow>
        <mo>=</mo>
        <mi>A</mi>
        <mi>B</mi>
        <msub>
          <mi>C</mi>
          <mrow class="MJX-TeXAtom-ORD">
            <mn>10..18</mn>
          </mrow>
        </msub>
      </mstyle>
    </mrow>
    <annotation encoding="application/x-tex">{\displaystyle {\frac {dABC\_{1..9}}{dt}}=ABC\_{10..18}}</annotation>
  </semantics>
</math></span><img src="https://wikimedia.org/api/rest_v1/media/math/render/svg/e7417e153de14d4837b22ca3fed992ca182e1a3a" class="mwe-math-fallback-image-inline mw-invert" aria-hidden="true" style="vertical-align: -2.005ex; width:22.714ex; height:5.509ex;" alt="{\displaystyle \frac{dABC\_{1..9}}{dt} = ABC\_{10..18}}"></span>



To keep things compact, we'll only display the first 20 lines of output.

```perl
# Simple Vector implementation
multi infix:<+>(@a, @b) { @a Z+ @b }
multi infix:<->(@a, @b) { @a Z- @b }
multi infix:<*>($r, @a) { $r X* @a }
multi infix:</>(@a, $r) { @a X/ $r }
sub norm { sqrt [+] @_ X** 2 }
 
# Runge-Kutta stuff
sub runge-kutta(&yp) {
    return -> \t, \y, \δt {
        my $a = δt * yp( t, y );
        my $b = δt * yp( t + δt/2, y + $a/2 );
        my $c = δt * yp( t + δt/2, y + $b/2 );
        my $d = δt * yp( t + δt, y + $c );
        ($a + 2*($b + $c) + $d) / 6;
    }
}
 
# gravitational constant
constant G = 6.674e-11;
# astronomical unit
constant au = 150e9;
 
# time constants in seconds
constant year = 365.25*24*60*60;
constant month = 21*24*60*60;
 
# masses in kg
constant $ma = 2e30;     # Sun
constant $mb = 6e24;     # Earth
constant $mc = 7.34e22;  # Moon

my &dABC = runge-kutta my &f = sub ( $t, @ABC ) {
    my @a = @ABC[0..2];
    my @b = @ABC[3..5];
    my @c = @ABC[6..8];
 
    my $ab = norm(@a - @b); 
    my $ac = norm(@a - @c);
    my $bc = norm(@b - @c);
 
    return [
        flat
        @ABC[@(9..17)],
        map G * *,
        $mb/$ab**3 * (@b - @a) + $mc/$ac**3 * (@c - @a),
        $ma/$ab**3 * (@a - @b) + $mc/$bc**3 * (@c - @b),
        $ma/$ac**3 * (@a - @c) + $mb/$bc**3 * (@b - @c);
    ];
}
 
loop (
    my ($t, @ABC) = 0,
        0, 0, 0,                                 # Sun position
        au, 0, 0,                                # Earth position
        0.998*au, 0, 0,                          # Moon position
        0, 0, 0,                                 # Sun speed
        0, 2*pi*au/year, 0,                      # Earth speed
        0, 2*pi*(au/year + 0.002*au/month), 0    # Moon speed
    ;
    $t < .2;
    ($t, @ABC) »+=« (.01, dABC($t, @ABC, .01))
) {
    printf "t = %.02f : %s\n", $t, @ABC.fmt("%+.3e");
}
```

#### Output:
```
t = 0.00 : +0.000e+00 +0.000e+00 +0.000e+00 +1.500e+11 +0.000e+00 +0.000e+00 +1.497e+11 +0.000e+00 +0.000e+00 +0.000e+00 +0.000e+00 +0.000e+00 +0.000e+00 +2.987e+04 +0.000e+00 +0.000e+00 +3.090e+04 +0.000e+00
t = 0.01 : +9.008e-13 +5.981e-22 +0.000e+00 +1.500e+11 +2.987e+02 +0.000e+00 +1.497e+11 +3.090e+02 +0.000e+00 +1.802e-10 +1.794e-19 +0.000e+00 -5.987e-05 +2.987e+04 +0.000e+00 -1.507e-05 +3.090e+04 +0.000e+00
t = 0.02 : +3.603e-12 +4.785e-21 +0.000e+00 +1.500e+11 +5.973e+02 +0.000e+00 +1.497e+11 +6.181e+02 +0.000e+00 +3.603e-10 +7.177e-19 +0.000e+00 -1.197e-04 +2.987e+04 +0.000e+00 -3.014e-05 +3.090e+04 +0.000e+00
t = 0.03 : +8.107e-12 +1.615e-20 +0.000e+00 +1.500e+11 +8.960e+02 +0.000e+00 +1.497e+11 +9.271e+02 +0.000e+00 +5.405e-10 +1.615e-18 +0.000e+00 -1.796e-04 +2.987e+04 +0.000e+00 -4.521e-05 +3.090e+04 +0.000e+00
t = 0.04 : +1.441e-11 +3.828e-20 +0.000e+00 +1.500e+11 +1.195e+03 +0.000e+00 +1.497e+11 +1.236e+03 +0.000e+00 +7.206e-10 +2.871e-18 +0.000e+00 -2.395e-04 +2.987e+04 +0.000e+00 -6.028e-05 +3.090e+04 +0.000e+00
t = 0.05 : +2.252e-11 +7.476e-20 +0.000e+00 +1.500e+11 +1.493e+03 +0.000e+00 +1.497e+11 +1.545e+03 +0.000e+00 +9.008e-10 +4.486e-18 +0.000e+00 -2.993e-04 +2.987e+04 +0.000e+00 -7.535e-05 +3.090e+04 +0.000e+00
t = 0.06 : +3.243e-11 +1.292e-19 +0.000e+00 +1.500e+11 +1.792e+03 +0.000e+00 +1.497e+11 +1.854e+03 +0.000e+00 +1.081e-09 +6.460e-18 +0.000e+00 -3.592e-04 +2.987e+04 +0.000e+00 -9.041e-05 +3.090e+04 +0.000e+00
t = 0.07 : +4.414e-11 +2.051e-19 +0.000e+00 +1.500e+11 +2.091e+03 +0.000e+00 +1.497e+11 +2.163e+03 +0.000e+00 +1.261e-09 +8.792e-18 +0.000e+00 -4.191e-04 +2.987e+04 +0.000e+00 -1.055e-04 +3.090e+04 +0.000e+00
t = 0.08 : +5.765e-11 +3.062e-19 +0.000e+00 +1.500e+11 +2.389e+03 +0.000e+00 +1.497e+11 +2.472e+03 +0.000e+00 +1.441e-09 +1.148e-17 +0.000e+00 -4.789e-04 +2.987e+04 +0.000e+00 -1.206e-04 +3.090e+04 +0.000e+00
t = 0.09 : +7.296e-11 +4.360e-19 +0.000e+00 +1.500e+11 +2.688e+03 +0.000e+00 +1.497e+11 +2.781e+03 +0.000e+00 +1.621e-09 +1.453e-17 +0.000e+00 -5.388e-04 +2.987e+04 +0.000e+00 -1.356e-04 +3.090e+04 +0.000e+00
t = 0.10 : +9.008e-11 +5.981e-19 +0.000e+00 +1.500e+11 +2.987e+03 +0.000e+00 +1.497e+11 +3.090e+03 +0.000e+00 +1.802e-09 +1.794e-17 +0.000e+00 -5.987e-04 +2.987e+04 +0.000e+00 -1.507e-04 +3.090e+04 +0.000e+00
t = 0.11 : +1.090e-10 +7.961e-19 +0.000e+00 +1.500e+11 +3.285e+03 +0.000e+00 +1.497e+11 +3.399e+03 +0.000e+00 +1.982e-09 +2.171e-17 +0.000e+00 -6.586e-04 +2.987e+04 +0.000e+00 -1.658e-04 +3.090e+04 +0.000e+00
t = 0.12 : +1.297e-10 +1.034e-18 +0.000e+00 +1.500e+11 +3.584e+03 +0.000e+00 +1.497e+11 +3.709e+03 +0.000e+00 +2.162e-09 +2.584e-17 +0.000e+00 -7.184e-04 +2.987e+04 +0.000e+00 -1.808e-04 +3.090e+04 +0.000e+00
t = 0.13 : +1.522e-10 +1.314e-18 +0.000e+00 +1.500e+11 +3.882e+03 +0.000e+00 +1.497e+11 +4.018e+03 +0.000e+00 +2.342e-09 +3.032e-17 +0.000e+00 -7.783e-04 +2.987e+04 +0.000e+00 -1.959e-04 +3.090e+04 +0.000e+00
t = 0.14 : +1.766e-10 +1.641e-18 +0.000e+00 +1.500e+11 +4.181e+03 +0.000e+00 +1.497e+11 +4.327e+03 +0.000e+00 +2.522e-09 +3.517e-17 +0.000e+00 -8.382e-04 +2.987e+04 +0.000e+00 -2.110e-04 +3.090e+04 +0.000e+00
t = 0.15 : +2.027e-10 +2.019e-18 +0.000e+00 +1.500e+11 +4.480e+03 +0.000e+00 +1.497e+11 +4.636e+03 +0.000e+00 +2.702e-09 +4.037e-17 +0.000e+00 -8.980e-04 +2.987e+04 +0.000e+00 -2.260e-04 +3.090e+04 +0.000e+00
t = 0.16 : +2.306e-10 +2.450e-18 +0.000e+00 +1.500e+11 +4.778e+03 +0.000e+00 +1.497e+11 +4.945e+03 +0.000e+00 +2.883e-09 +4.593e-17 +0.000e+00 -9.579e-04 +2.987e+04 +0.000e+00 -2.411e-04 +3.090e+04 +0.000e+00
t = 0.17 : +2.603e-10 +2.938e-18 +0.000e+00 +1.500e+11 +5.077e+03 +0.000e+00 +1.497e+11 +5.254e+03 +0.000e+00 +3.063e-09 +5.186e-17 +0.000e+00 -1.018e-03 +2.987e+04 +0.000e+00 -2.562e-04 +3.090e+04 +0.000e+00
t = 0.18 : +2.919e-10 +3.488e-18 +0.000e+00 +1.500e+11 +5.376e+03 +0.000e+00 +1.497e+11 +5.563e+03 +0.000e+00 +3.243e-09 +5.814e-17 +0.000e+00 -1.078e-03 +2.987e+04 +0.000e+00 -2.712e-04 +3.090e+04 +0.000e+00
t = 0.19 : +3.252e-10 +4.102e-18 +0.000e+00 +1.500e+11 +5.674e+03 +0.000e+00 +1.497e+11 +5.872e+03 +0.000e+00 +3.423e-09 +6.477e-17 +0.000e+00 -1.138e-03 +2.987e+04 +0.000e+00 -2.863e-04 +3.090e+04 +0.000e+00
```
