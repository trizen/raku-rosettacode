[1]: https://rosettacode.org/wiki/Generalised_floating_point_addition

# [Generalised floating point addition][1]

As long as all values are kept as rationals (type `Rat`) calculations are done full precision.

```raku
my $e = 63;
for -7..21 -> $n {
    my $num = '12345679' ~ '012345679' x ($n+7);
    my $sum = $_ + ($num * $_) * 81 given $e > -20 ?? 10**$e !! Rat.new(1,10**abs $e);
    printf "$n:%s ", 10**72 == $sum ?? 'Y' !! 'N';
    $e -= 9;
}
```

#### Output:
```
-7:Y -6:Y -5:Y -4:Y -3:Y -2:Y -1:Y 0:Y 1:Y 2:Y 3:Y 4:Y 5:Y 6:Y 7:Y 8:Y 9:Y 10:Y 11:Y 12:Y 13:Y 14:Y 15:Y 16:Y 17:Y 18:Y 19:Y 20:Y 21:Y
```
