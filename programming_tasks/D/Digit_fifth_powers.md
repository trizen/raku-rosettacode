[1]: https://rosettacode.org/wiki/Digit_fifth_powers

# [Digit fifth powers][1]

```perl
print q:to/EXPANATION/;
Sum of all integers (except 1 for some mysterious reason ¯\_(ツ)_/¯),
for which the individual digits to the nth power sum to itself.
EXPANATION
 
sub super($i) { $i.trans('0123456789' => '⁰¹²³⁴⁵⁶⁷⁸⁹') }
 
for 3..8 -> $power {
    print "\nSum of powers of n{super $power}: ";
    my $threshold = 9**$power * $power;
    put .join(' + '), ' = ', .sum with cache
    (2..$threshold).race.map: {
        state %p = ^10 .map: { $_ => $_ ** $power };
        $_ if %p{.comb}.sum == $_
    }
}
```

#### Output:
```
Sum of all integers (except 1 for some mysterious reason ¯\_(ツ)_/¯),
for which the individual digits to the nth power sum to itself.

Sum of powers of n³: 153 + 370 + 371 + 407 = 1301

Sum of powers of n⁴: 1634 + 8208 + 9474 = 19316

Sum of powers of n⁵: 4150 + 4151 + 54748 + 92727 + 93084 + 194979 = 443839

Sum of powers of n⁶: 548834 = 548834

Sum of powers of n⁷: 1741725 + 4210818 + 9800817 + 9926315 + 14459929 = 40139604

Sum of powers of n⁸: 24678050 + 24678051 + 88593477 = 137949578
```
