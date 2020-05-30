[1]: https://rosettacode.org/wiki/Esthetic_numbers

# [Esthetic numbers][1]

```perl
use Lingua::EN::Numbers;
 
sub esthetic($base = 10) {
    my @s = ^$base .map: -> \s {
        ((s - 1).base($base) if s > 0), ((s + 1).base($base) if s < $base - 1)
    }
 
    flat [ (1 .. $base - 1)».base($base) ],
    { [ flat .map: { $_ xx * Z~ flat @s[.comb.tail.parse-base($base)] } ] } … *
}
 
for 2 .. 16 -> $b {
    put "\n{(4 × $b).&ordinal-digit} through {(6 × $b).&ordinal-digit} esthetic numbers in base $b";
    put esthetic($b)[(4 × $b - 1) .. (6 × $b - 1)]».fmt('%3s').batch(16).join: "\n"
}
 
my @e10 = esthetic;
put "\nBase 10 esthetic numbers between 1,000 and 9,999:";
put @e10.&between(1000, 9999).batch(20).join: "\n";
 
put "\nBase 10 esthetic numbers between {1e8.Int.&comma} and {1.3e8.Int.&comma}:";
put @e10.&between(1e8.Int, 1.3e8.Int).batch(9).join: "\n";
 
sub between (@array, Int $lo, Int $hi) {
    my $top = @array.first: * > $hi, :k;
    @array[^$top].grep: * > $lo
}
```

#### Output:
```
8th through 12th esthetic numbers in base 2
10101010 101010101 1010101010 10101010101 101010101010

12th through 18th esthetic numbers in base 3
1210 1212 2101 2121 10101 10121 12101

16th through 24th esthetic numbers in base 4
323 1010 1012 1210 1212 1232 2101 2121 2123

20th through 30th esthetic numbers in base 5
323 343 432 434 1010 1012 1210 1212 1232 1234 2101

24th through 36th esthetic numbers in base 6
343 345 432 434 454 543 545 1010 1012 1210 1212 1232 1234

28th through 42nd esthetic numbers in base 7
345 432 434 454 456 543 545 565 654 656 1010 1012 1210 1212 1232

32nd through 48th esthetic numbers in base 8
432 434 454 456 543 545 565 567 654 656 676 765 767 1010 1012 1210
1212

36th through 54th esthetic numbers in base 9
434 454 456 543 545 565 567 654 656 676 678 765 767 787 876 878
1010 1012 1210

40th through 60th esthetic numbers in base 10
454 456 543 545 565 567 654 656 676 678 765 767 787 789 876 878
898 987 989 1010 1012

44th through 66th esthetic numbers in base 11
456 543 545 565 567 654 656 676 678 765 767 787 789 876 878 898
89A 987 989 9A9 A98 A9A 1010

48th through 72nd esthetic numbers in base 12
543 545 565 567 654 656 676 678 765 767 787 789 876 878 898 89A
987 989 9A9 9AB A98 A9A ABA BA9 BAB

52nd through 78th esthetic numbers in base 13
545 565 567 654 656 676 678 765 767 787 789 876 878 898 89A 987
989 9A9 9AB A98 A9A ABA ABC BA9 BAB BCB CBA

56th through 84th esthetic numbers in base 14
565 567 654 656 676 678 765 767 787 789 876 878 898 89A 987 989
9A9 9AB A98 A9A ABA ABC BA9 BAB BCB BCD CBA CBC CDC

60th through 90th esthetic numbers in base 15
567 654 656 676 678 765 767 787 789 876 878 898 89A 987 989 9A9
9AB A98 A9A ABA ABC BA9 BAB BCB BCD CBA CBC CDC CDE DCB DCD

64th through 96th esthetic numbers in base 16
654 656 676 678 765 767 787 789 876 878 898 89A 987 989 9A9 9AB
A98 A9A ABA ABC BA9 BAB BCB BCD CBA CBC CDC CDE DCB DCD DED DEF
EDC

Base 10 esthetic numbers between 1,000 and 9,999:
1010 1012 1210 1212 1232 1234 2101 2121 2123 2321 2323 2343 2345 3210 3212 3232 3234 3432 3434 3454
3456 4321 4323 4343 4345 4543 4545 4565 4567 5432 5434 5454 5456 5654 5656 5676 5678 6543 6545 6565
6567 6765 6767 6787 6789 7654 7656 7676 7678 7876 7878 7898 8765 8767 8787 8789 8987 8989 9876 9878
9898

Base 10 esthetic numbers between 100,000,000 and 130,000,000:
101010101 101010121 101010123 101012101 101012121 101012123 101012321 101012323 101012343
101012345 101210101 101210121 101210123 101212101 101212121 101212123 101212321 101212323
101212343 101212345 101232101 101232121 101232123 101232321 101232323 101232343 101232345
101234321 101234323 101234343 101234345 101234543 101234545 101234565 101234567 121010101
121010121 121010123 121012101 121012121 121012123 121012321 121012323 121012343 121012345
121210101 121210121 121210123 121212101 121212121 121212123 121212321 121212323 121212343
121212345 121232101 121232121 121232123 121232321 121232323 121232343 121232345 121234321
121234323 121234343 121234345 121234543 121234545 121234565 121234567 123210101 123210121
123210123 123212101 123212121 123212123 123212321 123212323 123212343 123212345 123232101
123232121 123232123 123232321 123232323 123232343 123232345 123234321 123234323 123234343
123234345 123234543 123234545 123234565 123234567 123432101 123432121 123432123 123432321
123432323 123432343 123432345 123434321 123434323 123434343 123434345 123434543 123434545
123434565 123434567 123454321 123454323 123454343 123454345 123454543 123454545 123454565
123454567 123456543 123456545 123456565 123456567 123456765 123456767 123456787 123456789
```