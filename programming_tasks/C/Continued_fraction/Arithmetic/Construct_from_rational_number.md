[1]: https://rosettacode.org/wiki/Continued_fraction/Arithmetic/Construct_from_rational_number

# [Continued fraction/Arithmetic/Construct from rational number][1]

Straightforward implementation:

```raku
sub r2cf(Rat $x is copy) {
    gather loop {
	$x -= take $x.floor;
	last unless $x > 0;
	$x = 1 / $x;
    }
}
 
say r2cf(.Rat) for <1/2 3 23/8 13/11 22/7 1.41 1.4142136>;
```

#### Output:
```
(0 2)
(3)
(2 1 7)
(1 5 2)
(3 7)
(1 2 2 3 1 1 2)
(1 2 2 2 2 2 2 2 2 2 6 1 2 4 1 1 2)
```


As a silly one-liner:

```raku
sub r2cf(Rat $x is copy) { gather $x [R/]= 1 while ($x -= take $x.floor) > 0 }
```