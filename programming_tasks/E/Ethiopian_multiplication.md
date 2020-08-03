[1]: https://rosettacode.org/wiki/Ethiopian_multiplication

# [Ethiopian multiplication][1]

```raku
sub halve  (Int $n is rw)    { $n div= 2 }
sub double (Int $n is rw)    { $n *= 2 }
sub even   (Int $n --> Bool) { $n %% 2 }
 
sub ethiopic-mult (Int $a is copy, Int $b is copy --> Int) {
    my Int $r = 0;
    while $a {
	even $a or $r += $b;
	halve $a;
	double $b;
    }
    return $r;
}
 
say ethiopic-mult(17,34);
```

#### Output:
```
578
```


More succinctly using implicit typing, primed lambdas, and an infinite loop:

```raku
sub ethiopic-mult {
    my &halve  = * div= 2;
    my &double = * *= 2;
    my &even   = * %% 2;
 
    my ($a,$b) = @_;
    my $r;
    loop {
        even  $a or $r += $b;
        halve $a or return $r;
        double $b;
    }
}
 
say ethiopic-mult(17,34);
```


More succinctly still, using a pure functional approach (reductions, mappings, lazy infinite sequences):

```raku
sub halve  { $^n div 2 }
sub double { $^n * 2   }
sub even   { $^n %% 2  }
 
sub ethiopic-mult ($a, $b) {
    [+] ($b, &double ... *)
        Z*
        ($a, &halve ... 0).map: { not even $^n }
}
 
say ethiopic-mult(17,34);
```