[1]: http://rosettacode.org/wiki/Circles_of_given_radius_through_two_points

# [Circles of given radius through two points][1]

```perl
sub circles(@A, @B where (not [and] @A Z== @B), $radius where * > 0) {
    my @middle = .5 X* (@A Z+ @B);
    my @diff = @A Z- @B;
    my @orth = -@diff[1], @diff[0] X/
    2 * tan asin 2*$radius R/ sqrt [+] @diff X**2;
 
    return (@middle Z+ @orth).item, (@middle Z- @orth).item;
}
 
my @input =
\([0.1234, 0.9876],  [0.8765, 0.2345],   2.0),
\([0.0000, 2.0000],  [0.0000, 0.0000],   1.0),
\([0.1234, 0.9876],  [0.1234, 0.9876],   2.0),
\([0.1234, 0.9876],  [0.8765, 0.2345],   0.5),
\([0.1234, 0.9876],  [0.1234, 0.9876],   0.0),
;
 
for @input -> $input {
    say $input.perl, ": ",
    try { say join " and ", circles(|$input) }
}
```

#### Output:
```
-0.863211801658189 -0.752111801658189 and 1.86311180165819 1.97421180165819
-6.12323399573677e-17 1 and 6.12323399573677e-17 1
NaN NaN and NaN NaN
```


Another possibility is to use the Complex plane,
for it often makes calculations easier with plane geometry:

```perl
sub circles($a, $b where $b != $a, $r) {
    my $h = ($b - $a)/2;
    my $l = sqrt($r**2 - $h.abs**2);
    return map { $a + $h + $l * $_ * $h/$h.abs },
    i, -i;
}
 
my @input =
\(0.1234 + 0.9876i,  0.8765 + 0.2345i,   2.0),
\(0.0000 + 2.0000i,  0.0000 + 0.0000i,   1.0),
\(0.1234 + 0.9876i,  0.1234 + 0.9876i,   2.0),
\(0.1234 + 0.9876i,  0.8765 + 0.2345i,   0.5),
\(0.1234 + 0.9876i,  0.1234 + 0.9876i,   0.0),
;
```