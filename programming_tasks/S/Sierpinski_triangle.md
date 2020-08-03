[1]: https://rosettacode.org/wiki/Sierpinski_triangle

# [Sierpinski triangle][1]

```perl
sub sierpinski ($n) {
    my @down  = '*';
    my $space = ' ';
    for ^$n {
        @down = |("$space$_$space" for @down), |("$_ $_" for @down);
        $space x= 2;
    }
    return @down;
}
 
.say for sierpinski 4;
```