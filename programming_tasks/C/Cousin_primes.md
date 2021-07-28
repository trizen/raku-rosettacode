[1]: https://rosettacode.org/wiki/Cousin_primes

# [Cousin primes][1]

### Filter



Favoring brevity over efficiency due to the small range of n, the most concise solution is:

```perl
say grep *.all.is-prime, map { $_, $_+4 }, 2..999;
```

#### Output:
```
((3 7) (7 11) (13 17) (19 23) (37 41) (43 47) (67 71) (79 83) (97 101) (103 107) (109 113) (127 131) (163 167) (193 197) (223 227) (229 233) (277 281) (307 311) (313 317) (349 353) (379 383) (397 401) (439 443) (457 461) (463 467) (487 491) (499 503) (613 617) (643 647) (673 677) (739 743) (757 761) (769 773) (823 827) (853 857) (859 863) (877 881) (883 887) (907 911) (937 941) (967 971))
```


### Infinite List



A more efficient and versatile approach is to generate an infinite list of cousin primes, using this info from [https://oeis.org/A023200](https://oeis.org/A023200)&#160;:

```perl
constant @cousins = (3, 7, *+6 … *).map: -> \n { (n, n+4) if (n & n+4).is-prime };
 
my $count = @cousins.first: :k, *.[0] > 1000;
 
.say for @cousins.head($count).batch(9);
```

#### Output:
```
((3 7) (7 11) (13 17) (19 23) (37 41) (43 47) (67 71) (79 83) (97 101))
((103 107) (109 113) (127 131) (163 167) (193 197) (223 227) (229 233) (277 281) (307 311))
((313 317) (349 353) (379 383) (397 401) (439 443) (457 461) (463 467) (487 491) (499 503))
((613 617) (643 647) (673 677) (739 743) (757 761) (769 773) (823 827) (853 857) (859 863))
((877 881) (883 887) (907 911) (937 941) (967 971))
```
