[1]: https://rosettacode.org/wiki/Unbias_a_random_generator

# [Unbias a random generator][1]



```perl
sub randN ( $n where 3..6 ) {
    return ( $n.rand / ($n - 1) ).Int;
}

sub unbiased ( $n where 3..6 ) {
    my $n1;
    repeat { $n1 = randN($n) } until $n1 != randN($n);
    return $n1;
}

my $iterations = 1000;
for 3 .. 6 -> $n {
    my ( @raw, @fixed );
    for ^$iterations {
        @raw[      randN($n) ]++;
        @fixed[ unbiased($n) ]++;
    }
    printf "N=%d   randN: %s, %4.1f%%   unbiased: %s, %4.1f%%\n",
        $n, map { .raku, .[1] * 100 / $iterations }, @raw, @fixed;
}
```

#### Output:
```
N=3   randN: [676, 324], 32.4%   unbiased: [517, 483], 48.3%
N=4   randN: [734, 266], 26.6%   unbiased: [489, 511], 51.1%
N=5   randN: [792, 208], 20.8%   unbiased: [494, 506], 50.6%
N=6   randN: [834, 166], 16.6%   unbiased: [514, 486], 48.6%
```
