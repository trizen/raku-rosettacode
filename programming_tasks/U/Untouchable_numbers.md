[1]: https://rosettacode.org/wiki/Untouchable_numbers

# [Untouchable numbers][1]

Borrow the proper divisors routine from [here](https://rosettacode.org/wiki/Proper_divisors#Raku).

```perl
# 20210220 Raku programming solution
 
sub propdiv (\x) {
   my @l = 1 if x > 1;
   (2 .. x.sqrt.floor).map: -> \d {
      unless x % d { @l.push: d; my \y = x div d; @l.push: y if y != d }
   }
   @l
}
 
sub sieve (\n) {
   my %s;
   for (0..(n+1)) -> \k {
      given ( [+] propdiv k ) { %s{$_} = True if $_ ≤ (n+1) }
   }
   %s;
}
 
my \limit = 1e5;
my %c = ( grep { !.is-prime }, 1..limit ).Set; # store composites
my %s = sieve(14 * limit);
my @untouchable = 2, 5;
 
loop ( my \n = $ = 6 ; n ≤ limit ; n += 2 ) {
   @untouchable.append(n) if (!%s{n} && %c{n-1} && %c{n-3})
}
 
my ($c, $last) = 0, False;  
for @untouchable.rotor(10) { 
   say [~] @_».&{$c++ ; $_ > 2000 ?? ( $last = True and last ) !! .fmt: "%6d "}
   $c-- and last if $last
}
 
say "\nList of untouchable numbers ≤ 2,000 : $c \n"; 
 
my ($p, $count) = 10,0;
BREAK: for @untouchable -> \n {
   $count++;
   if (n > $p) {
      printf "%6d untouchable numbers were found  ≤ %7d\n", $count-1, $p;
      last BREAK if limit == ($p *= 10)
   }
}
printf "%6d untouchable numbers were found  ≤ %7d\n", +@untouchable, limit
```

#### Output:
```
     2      5     52     88     96    120    124    146    162    188
   206    210    216    238    246    248    262    268    276    288
   290    292    304    306    322    324    326    336    342    372
   406    408    426    430    448    472    474    498    516    518
   520    530    540    552    556    562    576    584    612    624
   626    628    658    668    670    708    714    718    726    732
   738    748    750    756    766    768    782    784    792    802
   804    818    836    848    852    872    892    894    896    898
   902    926    934    936    964    966    976    982    996   1002
  1028   1044   1046   1060   1068   1074   1078   1080   1102   1116
  1128   1134   1146   1148   1150   1160   1162   1168   1180   1186
  1192   1200   1212   1222   1236   1246   1248   1254   1256   1258
  1266   1272   1288   1296   1312   1314   1316   1318   1326   1332
  1342   1346   1348   1360   1380   1388   1398   1404   1406   1418
  1420   1422   1438   1476   1506   1508   1510   1522   1528   1538
  1542   1566   1578   1588   1596   1632   1642   1650   1680   1682
  1692   1716   1718   1728   1732   1746   1758   1766   1774   1776
  1806   1816   1820   1822   1830   1838   1840   1842   1844   1852
  1860   1866   1884   1888   1894   1896   1920   1922   1944   1956
  1958   1960   1962   1972   1986   1992

List of untouchable numbers ≤ 2,000 : 196

     2 untouchable numbers were found  ≤      10
     5 untouchable numbers were found  ≤     100
    89 untouchable numbers were found  ≤    1000
  1212 untouchable numbers were found  ≤   10000
 13863 untouchable numbers were found  ≤  100000
```
