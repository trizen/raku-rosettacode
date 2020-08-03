[1]: https://rosettacode.org/wiki/Sorting_algorithms/Strand_sort

# [Sorting algorithms/Strand sort][1]

```perl
sub infix:<M> (@x-in, @y-in) {
    my @x = | @x-in;
    my @y = | @y-in;
    flat @x, @y,
        reverse gather while @x and @y {
            take do given @x[*-1] cmp @y[*-1] {
                when More { pop @x }
                when Less { pop @y }
                when Same { pop(@x), pop(@y) }
            }
        }
}
 
sub strand (@x) {
    my $i = 0;
    my $prev = -Inf;
    gather while $i < @x {
        @x[$i] before $prev ?? $i++ !! take $prev = splice(@x, $i, 1)[0];
    }
}
 
sub strand_sort (@x is copy) {
    my @out;
    @out M= strand(@x) while @x;
    @out;
}
 
my @a = (^100).roll(10);
say "Before {@a}";
@a = strand_sort(@a);
say "After  {@a}";
 
@a = <The quick brown fox jumps over the lazy dog>;
say "Before {@a}";
@a = strand_sort(@a);
say "After  {@a}";
```

#### Output:
```
Before 1 20 64 72 48 75 96 55 42 74
After  1 20 42 48 55 64 72 74 75 96
Before The quick brown fox jumps over the lazy dog
After  The brown dog fox jumps lazy over quick the
```