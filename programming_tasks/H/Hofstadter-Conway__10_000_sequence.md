[1]: http://rosettacode.org/wiki/Hofstadter-Conway_$10,000_sequence

# [Hofstadter-Conway $10,000 sequence][1]

Here is a list-oriented version. Note that `@a` is a lazy array, and the Z variants are "zipwith" operators.

```perl
my $n = 3;
my @a = (0,1,1, -> $p { @a[$p] + @a[$n++ - $p] } ... *);
 
my $last55;
for 1..19 -> $power {
    my @range := 2**$power .. 2**($power+1)-1;
    my @ratios = (@a[@range] Z/ @range) Z=> @range;
    my $max = @ratios.max;
    ($last55 = .value if .key >= .55 for @ratios) if $max.key >= .55;
    say $power.fmt('%2d'), @range.min.fmt("%10d"), '..', @range.max.fmt("%-10d"),
        $max.key, ' at ', $max.value;
}
say "Mallows' number would appear to be ", $last55;
```
```text
 1         2..3         0.666666666666667 at 3
 2         4..7         0.666666666666667 at 6
 3         8..15        0.636363636363636 at 11
 4        16..31        0.608695652173913 at 23
 5        32..63        0.590909090909091 at 44
 6        64..127       0.576086956521739 at 92
 7       128..255       0.567415730337079 at 178
 8       256..511       0.559459459459459 at 370
 9       512..1023      0.554937413073713 at 719
10      1024..2047      0.550100874243443 at 1487
11      2048..4095      0.547462892647566 at 2897
12      4096..8191      0.544144747863964 at 5969
13      8192..16383     0.542442708780362 at 11651
14     16384..32767     0.540071097511587 at 22223
15     32768..65535     0.538784020584256 at 45083
16     65536..131071    0.537043656999866 at 89516
17    131072..262143    0.536020067811561 at 181385
18    262144..524287    0.534645431078112 at 353683
19    524288..1048575   0.533779229963368 at 722589
Mallows' number would appear to be 1489
```


The lists are convenient, but here is a version written in relatively low-level primitives for performance:

```perl
my int $POW = 20;
my int $top = 2**$POW;
 
my int @a = (0,1,1);
@a[$top] = 0;   # pre-extend array
 
my int $n = 3;
my int $p = 1;
 
loop ($n = 3; $n <= $top; ++$n) {
    @a[$n] = $p = @a[$p] + @a[$n - $p];
}
 
my int $last55;
for 1 ..^ $POW -> int $power {
 
    my int $beg = 2 ** $power;
    my int $end = $beg * 2 - 1;
    my $max;
    my $ratio;
 
    loop (my $n = $beg; $n <= $end; ++$n) {
        my $ratio = @a[$n] / $n;
        $last55 = $n if $ratio * 100 >= 55;
        $max max= $ratio => $n;
    }
 
    say $power.fmt('%2d'), $beg.fmt("%10d"), '..', $end.fmt("%-10d"), $max.key, " at ", $max.value;
}
say "Mallows' number would appear to be ", $last55;
```