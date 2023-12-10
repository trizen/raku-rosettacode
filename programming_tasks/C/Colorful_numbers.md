[1]: https://rosettacode.org/wiki/Colorful_numbers

# [Colorful numbers][1]

```perl
sub is-colorful (Int $n) {
    return True if 0 <= $n <= 9;
    return False if $n.contains(0) || $n.contains(1) || $n < 0;
    my @digits = $n.comb;
    my %sums = @digits.Bag;
    return False if %sums.values.max > 1;
    for 2..@digits -> $group {
        @digits.rotor($group => 1 - $group).map: { %sums{ [×] $_ }++ }
        return False if %sums.values.max > 1;
    }
    True
}

put "Colorful numbers less than 100:\n" ~ (^100).hyper.grep( &is-colorful).batch(10)».fmt("%2d").join: "\n";

my ($start, $total) = 23456789, 10;

print "\nLargest magnitude colorful number: ";
.put and last if .Int.&is-colorful for $start.flip … $start;


put "\nCount of colorful numbers for each order of magnitude:\n" ~
    "1 digit colorful number count: $total - 100%";

for 2..8 {
   put "$_ digit colorful number count: ",
   my $c = +(flat $start.comb.combinations($_).map: {.permutations».join».Int}).hyper.grep( &is-colorful ),
   " - {($c / (exp($_,10) - exp($_-1,10) ) * 100).round(.001)}%";
   $total += $c;
}

say "\nTotal colorful numbers: $total";
```

#### Output:
```
Colorful numbers less than 100:
 0  1  2  3  4  5  6  7  8  9
23 24 25 26 27 28 29 32 34 35
36 37 38 39 42 43 45 46 47 48
49 52 53 54 56 57 58 59 62 63
64 65 67 68 69 72 73 74 75 76
78 79 82 83 84 85 86 87 89 92
93 94 95 96 97 98

Largest magnitude colorful number: 98746253

Count of colorful numbers for each order of magnitude:
1 digit colorful number count: 10 - 100%
2 digit colorful number count: 56 - 62.222%
3 digit colorful number count: 328 - 36.444%
4 digit colorful number count: 1540 - 17.111%
5 digit colorful number count: 5514 - 6.127%
6 digit colorful number count: 13956 - 1.551%
7 digit colorful number count: 21596 - 0.24%
8 digit colorful number count: 14256 - 0.016%

Total colorful numbers: 57256
```
