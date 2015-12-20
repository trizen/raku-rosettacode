[1]: http://rosettacode.org/wiki/Sorting_Algorithms/Circle_Sort

# [Sorting Algorithms/Circle Sort][1]

The given algorithm can be simplified in several ways. There's no need to compute the midpoint, since the hi/lo will end up there. The extra swap conditional can be eliminated by incrementing hi at the correct moment inside the loop. There's no need to
pass accumulated swaps down the call stack.



This does generic comparisons, so it works on any ordered type, including numbers or strings.

```perl
sub circlesort (@x is rw, $beg, $end) {
    my $swaps = 0;
    if $beg < $end {
        my ($lo, $hi) = $beg, $end;
        repeat {
            if @x[$lo] after @x[$hi] {
                @x[$lo,$hi] .= reverse;
                ++$swaps;
            }
            ++$hi if --$hi == ++$lo
        } while $lo < $hi;
        $swaps += circlesort(@x, $beg, $hi);
        $swaps += circlesort(@x, $lo, $end);
    }
    $swaps;
}
 
say my @x = (-100..100).roll(20);
say @x while circlesort(@x, 0, @x.end);
 
say @x = <The quick brown fox jumps over the lazy dog.>;
say @x while circlesort(@x, 0, @x.end);
```

#### Output:
```
16 35 -64 -29 46 36 -1 -99 20 100 59 26 76 -78 39 85 -7 -81 25 88
-99 -78 16 20 36 -81 -29 46 25 59 -64 -7 39 26 88 -1 35 85 76 100
-99 -78 -29 -81 16 -64 -7 20 -1 39 25 26 36 46 59 35 76 88 85 100
-99 -81 -78 -64 -29 -7 -1 16 20 25 26 35 36 39 46 59 76 85 88 100
The quick brown fox jumps over the lazy dog.
The brown fox jumps lazy dog. quick over the
The brown dog. fox jumps lazy over quick the
```