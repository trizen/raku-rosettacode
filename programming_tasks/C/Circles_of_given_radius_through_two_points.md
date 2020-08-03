[1]: https://rosettacode.org/wiki/Circles_of_given_radius_through_two_points

# [Circles of given radius through two points][1]

```perl
multi sub circles (@A, @B where ([and] @A Z== @B), 0.0) { 'Degenerate point' }
multi sub circles (@A, @B where ([and] @A Z== @B), $)   { 'Infinitely many share a point' }
multi sub circles (@A, @B, $radius) {
    my @middle = (@A Z+ @B) X/ 2;
    my @diff = @A Z- @B;
    my $q = sqrt [+] @diff X** 2;
    return 'Too far apart' if $q > $radius * 2;
 
    my @orth = -@diff[0], @diff[1] X* sqrt($radius ** 2 - ($q / 2) ** 2) / $q;
    return (@middle Z+ @orth), (@middle Z- @orth);
}
 
my @input =
    ([0.1234, 0.9876],  [0.8765, 0.2345],   2.0),
    ([0.0000, 2.0000],  [0.0000, 0.0000],   1.0),
    ([0.1234, 0.9876],  [0.1234, 0.9876],   2.0),
    ([0.1234, 0.9876],  [0.8765, 0.2345],   0.5),
    ([0.1234, 0.9876],  [0.1234, 0.9876],   0.0),
    ;
 
for @input {
    say .list.perl, ': ', circles(|$_).join(' and ');
}
```

#### Output:
```
([0.1234, 0.9876], [0.8765, 0.2345], 2.0): 1.86311180165819 1.97421180165819 and -0.863211801658189 -0.752111801658189
([0.0, 2.0], [0.0, 0.0], 1.0): 0 1 and 0 1
([0.1234, 0.9876], [0.1234, 0.9876], 2.0): Infinitely many share a point
([0.1234, 0.9876], [0.8765, 0.2345], 0.5): Too far apart
([0.1234, 0.9876], [0.1234, 0.9876], 0.0): Degenerate point
```


Another possibility is to use the Complex plane,
for it often makes calculations easier with plane geometry:

```perl
multi sub circles ($a, $b where $a == $b, 0.0) { 'Degenerate point' }
multi sub circles ($a, $b where $a == $b, $)   { 'Infinitely many share a point' }
multi sub circles ($a, $b, $r) {
    my $h = ($b - $a) / 2;
    my $l = sqrt($r**2 - $h.abs**2);
    return 'Too far apart' if $l.isNaN;
    return map { $a + $h + $l * $_ * $h / $h.abs }, i, -i;
}
 
my @input =
    (0.1234 + 0.9876i,  0.8765 + 0.2345i,   2.0),
    (0.0000 + 2.0000i,  0.0000 + 0.0000i,   1.0),
    (0.1234 + 0.9876i,  0.1234 + 0.9876i,   2.0),
    (0.1234 + 0.9876i,  0.8765 + 0.2345i,   0.5),
    (0.1234 + 0.9876i,  0.1234 + 0.9876i,   0.0),
    ;
 
for @input {
    say .join(', '), ': ', circles(|$_).join(' and ');
}
```

#### Output:
```
0.1234+0.9876i, 0.8765+0.2345i, 2: 1.86311180165819+1.97421180165819i and -0.863211801658189-0.752111801658189i
0+2i, 0+0i, 1: 0+1i and 0+1i
0.1234+0.9876i, 0.1234+0.9876i, 2: Infinitely many share a point
0.1234+0.9876i, 0.8765+0.2345i, 0.5: Too far apart
0.1234+0.9876i, 0.1234+0.9876i, 0: Degenerate point
```