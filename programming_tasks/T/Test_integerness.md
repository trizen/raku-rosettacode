[1]: https://rosettacode.org/wiki/Test_integerness

# [Test integerness][1]





In Raku, all numeric types have a method called `narrow`, which returns an object with the same value but of the most appropriate type.  So we can just check if *that* object is an `Int`. This works even with floats with large exponents, because the `Int` type supports arbitrarily large integers.



For the extra credit task, we can add another multi candidate that checks the distance between the number and it's nearest integer, but then we'll have to handle complex numbers specially.

```perl
multi is-int ($n) { $n.narrow ~~ Int }

multi is-int ($n, :$tolerance!) {
    abs($n.round - $n) <= $tolerance
}

multi is-int (Complex $n, :$tolerance!) {
    is-int($n.re, :$tolerance) && abs($n.im) < $tolerance
}

# Testing:

for 25.000000, 24.999999, 25.000100, -2.1e120, -5e-2, Inf, NaN, 5.0+0.0i, 5-5i {
    printf "%-7s  %-9s  %-5s  %-5s\n", .^name, $_,
        is-int($_),
        is-int($_, :tolerance<0.00001>);
}
```

#### Output:
```
Rat      25         True   True 
Rat      24.999999  False  True 
Rat      25.0001    False  False
Num      -2.1e+120  True   True 
Num      -0.05      False  False
Num      Inf        False  False
Num      NaN        False  False
Complex  5+0i       True   True 
Complex  5-5i       False  False
```
