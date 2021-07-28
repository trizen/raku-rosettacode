[1]: https://rosettacode.org/wiki/Modified_random_distribution

# [Modified random distribution][1]

```perl
sub modified_random_distribution ( Code $modifier --> Seq ) {
    return lazy gather loop {
        my ( $r1, $r2 ) = rand, rand;
        take $r1 if $modifier.($r1) > $r2;
    }
}
sub modifier ( Numeric $x --> Numeric ) {
    return 2 * ( $x < 1/2 ?? ( 1/2 - $x  )
                          !! ( $x  - 1/2 ) );
} 
sub print_histogram ( @data, :$n-bins, :$width ) { # Assumes minimum of zero.
    my %counts = bag @data.map: { floor( $_ * $n-bins ) / $n-bins };
    my $max_value = %counts.values.max;
    sub hist { '■' x ( $width * $^count / $max_value ) }
    say ' Bin, Counts: Histogram';
    printf "%4.2f, %6d: %s\n", .key, .value, hist(.value) for %counts.sort;
}
 
my @d = modified_random_distribution( &modifier );
 
print_histogram( @d.head(50_000), :n-bins(20), :width(64) );
```

#### Output:
```
 Bin, Counts: Histogram
0.00,   4718: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.05,   4346: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.10,   3685: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.15,   3246: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.20,   2734: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.25,   2359: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.30,   1702: ■■■■■■■■■■■■■■■■■■■■■■
0.35,   1283: ■■■■■■■■■■■■■■■■■
0.40,    702: ■■■■■■■■■
0.45,    250: ■■■
0.50,    273: ■■■
0.55,    745: ■■■■■■■■■■
0.60,   1231: ■■■■■■■■■■■■■■■■
0.65,   1757: ■■■■■■■■■■■■■■■■■■■■■■■
0.70,   2209: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.75,   2738: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.80,   3255: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.85,   3741: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.90,   4268: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
0.95,   4758: ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■
```
