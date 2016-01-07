[1]: http://rosettacode.org/wiki/Convert_decimal_number_to_rational

# [Convert decimal number to rational][1]

Decimals are natively represented as rationals in Perl 6, so if the task does not need to handle repeating decimals, it is trivially handled by the `.nude` method, which returns the numerator and denominator:

```perl
say .nude.join('/') for 0.9054054, 0.518518, 0.75;
```

#### Output:
```
4527027/5000000
259259/500000
3/4
```


However, if we want to take repeating decimals into account, then we can get a bit fancier.

```perl
sub decimal_to_fraction ( Str $n, Int $rep_digits = 0 ) returns Str {
    my ( $int, $dec ) = ( $n ~~ /^ (\d+) \. (\d+) $/ )».Str or die;
 
    my ( $numer, $denom ) = ( $dec, 10 ** $dec.chars );
    if $rep_digits {
        my $to_move = $dec.chars - $rep_digits;
        $numer -= $dec.substr(0, $to_move);
        $denom -= 10 ** $to_move;
    }
 
    my $rat = Rat.new( $numer.Int, $denom.Int ).nude.join('/');
    return $int > 0 ?? "$int $rat" !! $rat;
}
 
my @a = ['0.9054', 3], ['0.518', 3], ['0.75', 0], | (^4).map({['12.34567', $_]});
for @a -> [ $n, $d ] {
    say "$n with $d repeating digits = ", decimal_to_fraction( $n, $d );
}
```

#### Output:
```
0.9054 with 3 repeating digits = 67/74
0.518 with 3 repeating digits = 14/27
0.75 with 0 repeating digits = 3/4
12.34567 with 0 repeating digits = 12 34567/100000
12.34567 with 1 repeating digits = 12 31111/90000
12.34567 with 2 repeating digits = 12 17111/49500
12.34567 with 3 repeating digits = 12 1279/3700
```