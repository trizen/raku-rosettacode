[1]: https://rosettacode.org/wiki/Arithmetic-geometric_mean

# [Arithmetic-geometric mean][1]



```perl
sub agm( $a is copy, $g is copy ) {
    ($a, $g) = ($a + $g)/2, sqrt $a * $g until $a ≅ $g;
    return $a;
}
 
say agm 1, 1/sqrt 2;
```

#### Output:
```
0.84721308479397917
```


It's also possible to write it recursively:

```perl
sub agm( $a, $g ) {
    $a ≅ $g ?? $a !! agm(|@$_)
        given ($a + $g)/2, sqrt $a * $g;
}

say agm 1, 1/sqrt 2;
```


We can also get a bit fancy and use a converging sequence of complex numbers:

```perl
sub agm {
  ($^z, {(.re+.im)/2 + (.re*.im).sqrt*1i} ... * ≅ *)
  .tail.re
}
 
say agm 1 + 1i/2.sqrt
```
