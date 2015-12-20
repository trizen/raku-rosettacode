[1]: http://rosettacode.org/wiki/Roots_of_unity

# [Roots of unity][1]

Perl 6 has a built-in function <tt>cis</tt> which returns a unitary complex number given its phase. Perl 6 also defines the <tt>tau = 2\*pi</tt> constant. Thus the k-th n-root of unity can simply be written <tt>cis(k\*τ/n)&lt;tt&gt;.

```perl6
constant n = 10;
for ^n -> \k {
    say cis(k*τ/n);
}
```

#### Output:
```
1+0i
0.809016994374947+0.587785252292473i
0.309016994374947+0.951056516295154i
-0.309016994374947+0.951056516295154i
-0.809016994374947+0.587785252292473i
-1+1.22464679914735e-16i
-0.809016994374948-0.587785252292473i
-0.309016994374948-0.951056516295154i
0.309016994374947-0.951056516295154i
0.809016994374947-0.587785252292473i
```