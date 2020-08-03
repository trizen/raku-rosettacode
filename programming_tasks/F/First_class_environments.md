[1]: https://rosettacode.org/wiki/First_class_environments

# [First class environments][1]

Fairly straightforward. Set up an array of hashes containing the current values and iteration counts then pass each hash in turn with a code reference to a routine to calculate the next iteration.

```raku
my $calculator = sub ($n is rw) {
    return ($n == 1) ?? 1 !! $n %% 2 ?? $n div 2 !! $n * 3 + 1
};
 
sub next (%this, &get_next) {
    return %this if %this.<value> == 1;
    %this.<value>.=&get_next;
    %this.<count>++;
    return %this;
};
 
my @hailstones = map { %(value => $_, count => 0) }, 1 .. 12;
 
while not all( map { $_.<value> }, @hailstones ) == 1 {
    say [~] map { $_.<value>.fmt("%4s") }, @hailstones;
    @hailstones[$_].=&next($calculator) for ^@hailstones;
}
 
say 'Counts';
 
say [~] map { $_.<count>.fmt("%4s") }, @hailstones;
```

#### Output:
```
   1   2   3   4   5   6   7   8   9  10  11  12
   1   1  10   2  16   3  22   4  28   5  34   6
   1   1   5   1   8  10  11   2  14  16  17   3
   1   1  16   1   4   5  34   1   7   8  52  10
   1   1   8   1   2  16  17   1  22   4  26   5
   1   1   4   1   1   8  52   1  11   2  13  16
   1   1   2   1   1   4  26   1  34   1  40   8
   1   1   1   1   1   2  13   1  17   1  20   4
   1   1   1   1   1   1  40   1  52   1  10   2
   1   1   1   1   1   1  20   1  26   1   5   1
   1   1   1   1   1   1  10   1  13   1  16   1
   1   1   1   1   1   1   5   1  40   1   8   1
   1   1   1   1   1   1  16   1  20   1   4   1
   1   1   1   1   1   1   8   1  10   1   2   1
   1   1   1   1   1   1   4   1   5   1   1   1
   1   1   1   1   1   1   2   1  16   1   1   1
   1   1   1   1   1   1   1   1   8   1   1   1
   1   1   1   1   1   1   1   1   4   1   1   1
   1   1   1   1   1   1   1   1   2   1   1   1
Counts
   0   1   7   2   5   8  16   3  19   6  14   9
```