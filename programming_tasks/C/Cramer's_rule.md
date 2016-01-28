[1]: http://rosettacode.org/wiki/Cramer's_rule

# [Cramer's rule][1]

```perl
sub det(@A) {
    [+] gather for ^@A -> $i {
        my ($p1, $p2) = (1, 1);
        for ^@A[$i] -> $j {
            $p1 *= @A[($j+$i)%@A][$j];
            $p2 *= @A[($j+$i)%@A][*-$j-1];
        }
        take $p1-$p2;
    }
}
 
sub cramers_rule(@A, @terms) {
    gather for ^@A -> $i {
        my @Ai = @A.map: { [|$_] };
        for ^@terms -> $j {
            @Ai[$j][$i] = @terms[$j];
        }
        take det(@Ai);
    } »/» det(@A);
}
 
my @matrix = (
    [2, -3,  1],
    [1, -2, -2],
    [3, -4,  1],
);
 
my @free_terms = (4, -6, 5);
my ($x, $y, $z) = |cramers_rule(@matrix, @free_terms);
 
say "x = $x";
say "y = $y";
say "z = $z";
```

#### Output:
```
x = 2
y = 1
z = 3
```