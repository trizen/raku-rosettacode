[1]: https://rosettacode.org/wiki/Stirling_numbers_of_the_second_kind

# [Stirling numbers of the second kind][1]

```perl
sub Stirling2 (Int \n, Int \k) {
    ((1,), { (0, |@^last) »+« (|(@^last »*« @^last.keys), 0) } … *)[n;k]
}
 
my $upto = 12;
 
my $mx = (1..^$upto).map( { Stirling2($upto, $_) } ).max.chars;
 
put 'Stirling numbers of the second kind: S2(n, k):';
put 'n\k', (0..$upto)».fmt: "%{$mx}d";
 
for 0..$upto -> $row {
    $row.fmt('%-3d').print;
    put (0..$row).map( { Stirling2($row, $_) } )».fmt: "%{$mx}d";
}
 
say "\nMaximum value from the S2(100, *) row:";
say (^100).map( { Stirling2 100, $_ } ).max;
```

#### Output:
```
Stirling numbers of the second kind: S2(n, k):
n\k      0       1       2       3       4       5       6       7       8       9      10      11      12
0        1
1        0       1
2        0       1       1
3        0       1       3       1
4        0       1       7       6       1
5        0       1      15      25      10       1
6        0       1      31      90      65      15       1
7        0       1      63     301     350     140      21       1
8        0       1     127     966    1701    1050     266      28       1
9        0       1     255    3025    7770    6951    2646     462      36       1
10       0       1     511    9330   34105   42525   22827    5880     750      45       1
11       0       1    1023   28501  145750  246730  179487   63987   11880    1155      55       1
12       0       1    2047   86526  611501 1379400 1323652  627396  159027   22275    1705      66       1

Maximum value from the S2(100, *) row:
7769730053598745155212806612787584787397878128370115840974992570102386086289805848025074822404843545178960761551674
```
