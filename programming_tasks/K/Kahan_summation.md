[1]: https://rosettacode.org/wiki/Kahan_summation

# [Kahan summation][1]

Perl&#160;6 does not offer a fixed precision decimal. It *does* have IEEE 754 floating point numbers so let's try implementing the floating point option as shown in Python. Need to explicitly specify scientific notation numbers to force floating point Nums.

```raku
constant ε = (1e0, */2e0 … *+1e0==1e0)[*-1];
 
sub kahan (*@nums) {
    my $summ = my $c = 0e0;
    for @nums -> $num {
        my $y = $num - $c;
        my $t = $summ + $y;
        $c = ($t - $summ) - $y;
        $summ = $t;
    }
    $summ
}
 
say 'Epsilon:    ', ε;
 
say 'Simple sum: ', ((1e0 + ε) - ε).fmt: "%.16f";
 
say 'Kahan sum:  ', kahan(1e0, ε, -ε).fmt: "%.16f";
```

#### Output:
```
Epsilon:    1.1102230246251565e-16
Simple sum: 0.9999999999999999
Kahan sum:  1.0000000000000000
```