[1]: https://rosettacode.org/wiki/Constrained_random_points_on_a_circle

# [Constrained random points on a circle][1]

```perl
my @range = -15..16;
 
my @points = gather for @range X @range -> ($x, $y) {
    take [$x,$y] if 10 <= sqrt($x*$x+$y*$y) <= 15
}
my @samples = @points.roll(100); # or .pick(100) to get distinct points
 
# format and print
my %matrix;
for @range X @range -> ($x, $y) { %matrix{$y}{$x} = ' ' }
%matrix{$_[1]}{$_[0]} = '*' for @samples;
%matrix{$_}{@range}.join(' ').say for @range;
```


Turning that program completely inside-out and reducing to a single statement with a single non-parameter variable, we get this version, which also works:

```perl
(say ~.map: { $_ // ' ' } for my @matrix) given do
    -> [$x, $y] { @matrix[$x][$y] = '*' } for pick 100, do
        for ^32 X ^32 -> ($x, $y) {
            [$x,$y] when 100..225 given [+] ($x,$y X- 15) X** 2;
        }
 
```


This uses, among other things, a 0-based matrix rather than a hash, a `given` on the first line that allows us to print the final value of the matrix straight from its initial declaration, a `for` statement feeding a `for` statement modifier, a lambda that unpacks a single x-y argument into two variables, the functional form of pick rather than the method form, a quasi-list comprehension in the middle loop that filters each `given` with a `when`, precalculated squared limits so we don't have to take the square root, use of X- and X\*\* to subtract and exponentiate both `$x` and `$y` in parallel.



After the `given do` has loaded up `@matrix` with our circle, the `map` on the first line substitutes a space for any undefined matrix element, and the extra space between elements is supplied by the stringification of the list value, performed by the prefix `~` operator, the unary equivalent of concatenation in Perl&#160;6.



At this point you would be justified in concluding that we are completely mad. `:-)`