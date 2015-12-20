[1]: http://rosettacode.org/wiki/Horner's_rule_for_polynomial_evaluation

# [Horner's rule for polynomial evaluation][1]

```perl6
sub horner ( @coeffs, $x ) {
    @coeffs.reverse.reduce: { $^a * $x + $^b };
}
 
say horner( [ -19, 7, -4, 6 ], 3 );
```


A recursive version would spare us the need for reversing the list of coefficients. However, special care must be taken in order to write it, because the way Perl 6 implements lists is not optimized for this kind of treatment. [Lisp](/wiki/Lisp" title="Lisp" class="mw-redirect)-style lists are, and fortunately it is possible to emulate them with [Pairs](http://doc.perl6.org/type/Pair) and the reduction meta-operator:

```perl6
multi horner(Numeric $c, $) { $c }
multi horner(Pair $c, $x) {
    $c.key + $x * horner( $c.value, $x ) 
}
 
say horner( [=>](-19, 7, -4, 6 ), 3 );
```


We can also use the composition operator:

```perl6
sub horner ( @coeffs, $x ) {
    ([o] map { $_ + $x * * }, @coeffs)(0);
}
 
say horner( [ -19, 7, -4, 6 ], 3 );
```

#### Output:
```
128
```


One advantage of using the composition operator is that it allows for the use of an infinite list of coefficients.

```perl6
sub horner ( @coeffs, $x ) {
    map { .(0) }, [\o] map { $_ + $x * * }, @coeffs;
}
 
say horner( [ 1 X/ (1, |[\*] 1 .. *) ], i*pi )[20];
 
```

#### Output:
```
-0.999999999924349-5.28918515954219e-10i
```