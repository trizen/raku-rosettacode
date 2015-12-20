[1]: http://rosettacode.org/wiki/Arithmetic-geometric_mean

# [Arithmetic-geometric mean][1]

```perl
 
sub agm( $a is copy, $g is copy ) {
    loop {
        given ($a + $g)/2, sqrt $a * $g {
            return $a if @$_ ~~ ($a, $g);
            ($a, $g) = @$_;
        }
    }
}
 
say agm 1, 1/sqrt 2;
 
 
```

#### Output:
```
0.84721308479397917
```


Obviously the "fixed point" detector here is relying on the floating-point representation running out of bits, or this algorithm would not terminate before using up all memory.



It's also possible to write it recursively:

```perl
 
sub agm( $a, $g ) {
    @$_ ~~ ($a, $g) ?? $a !! agm(|@$_)
        given ($a + $g)/2, [sqrt](http://perldoc.perl.org/functions/sqrt.html) $a * $g;
}
 
say agm 1, 1/[sqrt](http://perldoc.perl.org/functions/sqrt.html) 2;
```