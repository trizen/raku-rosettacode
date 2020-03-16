[1]: https://rosettacode.org/wiki/Next_highest_int_from_digits

# [Next highest int from digits][1]




Minimal error trapping. Assumes that the passed number is an integer. Handles positive or negative integers, always returns next largest regardless (if possible).

```perl
use Lingua::EN::Numbers;
 
sub next-greatest-index ($str, &op = &infix:«<» ) {
    my @i = $str.comb;
    (1..^@i).first: { &op(@i[$_ - 1], @i[$_]) }, :end, :k;
}
 
multi next-greatest-integer (Int $num where * >= 0) {
    return 0 if $num.chars < 2;
    return $num.flip > $num ?? $num.flip !! 0 if $num.chars == 2;
    return 0 unless my $i = next-greatest-index( $num ) // 0;
    my $digit = $num.substr($i, 1);
    my @rest  = (flat $num.substr($i).comb).sort(+*);
    my $next  = @rest.first: * > $digit, :k;
    $digit    = @rest.splice($next,1);
    join '', flat $num.substr(0,$i), $digit, @rest;
}
 
multi next-greatest-integer (Int $num where * < 0) {
    return 0 if $num.chars < 3;
    return $num.abs.flip < -$num ?? -$num.abs.flip !! 0 if $num.chars == 3;
    return 0 unless my $i = next-greatest-index( $num, &CORE::infix:«>» ) // 0;
    my $digit = $num.substr($i, 1);
    my @rest  = (flat $num.substr($i).comb).sort(-*);
    my $next  = @rest.first: * < $digit, :k;
    $digit    = @rest.splice($next,1);
    join '', flat $num.substr(0,$i), $digit, @rest;
}
 
say "Next largest integer able to be made from these digits, or zero if no larger exists:";
printf "%30s  -> %s%s\n", .&comma, .&next-greatest-integer < 0 ?? '' !! ' ', .&next-greatest-integer.&comma for
    flat 0, (9, 12, 21, 12453, 738440, 45072010, 95322020, 9589776899767587796600, 3345333,
    95897768997675877966000000000000000000000000000000000000000000000000000000000000000000).map: { $_, -$_ };
```

#### Output:
```
Next largest integer able to be made from these digits, or zero if no larger exists:
                             0  ->  0
                             9  ->  0
                            -9  ->  0
                            12  ->  21
                           -12  ->  0
                            21  ->  0
                           -21  -> -12
                        12,453  ->  12,534
                       -12,453  -> -12,435
                       738,440  ->  740,348
                      -738,440  -> -738,404
                    45,072,010  ->  45,072,100
                   -45,072,010  -> -45,072,001
                    95,322,020  ->  95,322,200
                   -95,322,020  -> -95,322,002
 9,589,776,899,767,587,796,600  ->  9,589,776,899,767,587,900,667
-9,589,776,899,767,587,796,600  -> -9,589,776,899,767,587,796,060
                     3,345,333  ->  3,353,334
                    -3,345,333  -> -3,343,533
95,897,768,997,675,877,966,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000  ->  95,897,768,997,675,879,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,667
-95,897,768,997,675,877,966,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000  -> -95,897,768,997,675,877,960,600,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000,000
```
