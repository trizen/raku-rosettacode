[1]: https://rosettacode.org/wiki/Lah_numbers

# [Lah numbers][1]

```perl
constant @factorial = 1, |[\*] 1..*;
 
sub Lah (Int \n, Int \k) {
    return @factorial[n] if k == 1;
    return 1 if k == n;
    return 0 if k > n;
    return 0 if k < 1 or n < 1;
    (@factorial[n] * @factorial[n - 1]) / (@factorial[k] * @factorial[k - 1]) / @factorial[n - k]
}
 
my $upto = 12;
 
my $mx = (1..$upto).map( { Lah($upto, $_) } ).max.chars;
 
put 'Unsigned Lah numbers:  L(n, k):';
put 'n\k', (0..$upto)».fmt: "%{$mx}d";
 
for 0..$upto -> $row {
    $row.fmt('%-3d').print;
    put (0..$row).map( { Lah($row, $_) } )».fmt: "%{$mx}d";
}
 
say "\nMaximum value from the L(100, *) row:";
say (^100).map( { Lah 100, $_ } ).max;
```

#### Output:
```
Unsigned Lah numbers:  L(n, k):
n\k         0          1          2          3          4          5          6          7          8          9         10         11         12
0           1
1           0          1
2           0          2          1
3           0          6          6          1
4           0         24         36         12          1
5           0        120        240        120         20          1
6           0        720       1800       1200        300         30          1
7           0       5040      15120      12600       4200        630         42          1
8           0      40320     141120     141120      58800      11760       1176         56          1
9           0     362880    1451520    1693440     846720     211680      28224       2016         72          1
10          0    3628800   16329600   21772800   12700800    3810240     635040      60480       3240         90          1
11          0   39916800  199584000  299376000  199584000   69854400   13970880    1663200     118800       4950        110          1
12          0  479001600 2634508800 4390848000 3293136000 1317254400  307359360   43908480    3920400     217800       7260        132          1

Maximum value from the L(100, *) row:
44519005448993144810881324947684737529186447692709328597242209638906324913313742508392928375354932241404408343800007105650554669129521241784320000000000000000000000
```
