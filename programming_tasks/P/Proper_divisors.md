[1]: http://rosettacode.org/wiki/Proper_divisors

# [Proper divisors][1]

```perl
sub propdiv (\x) {
    my @l =(1 if x > 1), gather for 2 .. x.sqrt.floor -> \d {
        my \y = x div d;
        if y * d == x { take d; take y unless y == d }
    }
    gather @l.deepmap(*.take);
}
 
say "$_ ", propdiv($_) for 1..10;
 
my $max = 0;
my @candidates;
for 1..20000 {
    my @pd = propdiv($_);
    my $pd = @pd.elems;
    if $pd > $max {
        @candidates = ();
        $max = $pd;
    }
    push @candidates, $_ if $pd == $max;
}
say "max = $max, candidates = @candidates[]";
```

#### Output:
```
1       Nil 
2       1 
3       1 
4       1 2
5       1 
6       1 2 3
7       1 
8       1 2 4
9       1 3
10      1 2 5
max = 79, candidates = 15120 18480
```