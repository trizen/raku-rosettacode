[1]: http://rosettacode.org/wiki/Aliquot_sequence_classifications

# [Aliquot sequence classifications][1]

```perl
sub propdivsum (\x) {
    [+] x > 1, gather for 2 .. x.sqrt.floor -> \d {
        my \y = x div d;
        if y * d == x { take d; take y unless y == d }
    }
}
 
multi quality (0,1)  { 'perfect ' }
multi quality (0,2)  { 'amicable' }
multi quality (0,$n) { "sociable-$n" }
multi quality ($,1)  { 'aspiring' }
multi quality ($,$n) { "cyclic-$n" }
 
sub aliquotidian ($x) {
    my %seen;
    my @seq = $x, &propdivsum ... *;
    for 0..16 -> $to {
        my $this = @seq[$to] or return "$x\tterminating\t[@seq[^$to]]";
        last if $this > 140737488355328;
        if %seen{$this}:exists {
            my $from = %seen{$this};
            return "$x\t&quality($from, $to-$from)\t[@seq[^$to]]";
        }
        %seen{$this} = $to;
    }
    "$x non-terminating";
 
}
 
aliquotidian($_).say for flat
    1..10,
    11, 12, 28, 496, 220, 1184, 12496, 1264460,
    790, 909, 562, 1064, 1488,
    15355717786080;
```

#### Output:
```
1       terminating     [1]
2       terminating     [2 1]
3       terminating     [3 1]
4       terminating     [4 3 1]
5       terminating     [5 1]
6       perfect         [6]
7       terminating     [7 1]
8       terminating     [8 7 1]
9       terminating     [9 4 3 1]
10      terminating     [10 8 7 1]
11      terminating     [11 1]
12      terminating     [12 16 15 9 4 3 1]
28      perfect         [28]
496     perfect         [496]
220     amicable        [220 284]
1184    amicable        [1184 1210]
12496   sociable-5      [12496 14288 15472 14536 14264]
1264460 sociable-4      [1264460 1547860 1727636 1305184]
790     aspiring        [790 650 652 496]
909     aspiring        [909 417 143 25 6]
562     cyclic-2        [562 284 220]
1064    cyclic-2        [1064 1336 1184 1210]
1488    non-terminating
15355717786080  non-terminating
```