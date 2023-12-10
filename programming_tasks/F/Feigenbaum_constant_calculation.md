[1]: https://rosettacode.org/wiki/Feigenbaum_constant_calculation

# [Feigenbaum constant calculation][1]



```perl
my $a1 = 1;
my $a2 = 0;
my $d = 3.2;

say ' i d';

for 2 .. 13 -> $exp {
    my $a = $a1 + ($a1 - $a2) / $d;
    do {
        my $x = 0;
        my $y = 0;
        for ^2 ** $exp {
            $y = 1 - 2 * $y * $x;
            $x = $a - $x²;
        }
        $a -= $x / $y;
    } xx 10;
     $d = ($a1 - $a2) / ($a - $a1);
     ($a2, $a1) = ($a1, $a);
     printf "%2d %.8f\n", $exp, $d;
}
```

#### Output:
```
 i d
 2 3.21851142
 3 4.38567760
 4 4.60094928
 5 4.65513050
 6 4.66611195
 7 4.66854858
 8 4.66906066
 9 4.66917155
10 4.66919515
11 4.66920026
12 4.66920098
13 4.66920537
```
