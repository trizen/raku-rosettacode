[1]: https://rosettacode.org/wiki/Primes_whose_first_and_last_number_is_3

# [Primes whose first and last number is 3][1]

```perl
my $upto = 1e4;

for 1,3,5,7,9 -> $bracket {
    print "\nPrimes up to $upto bracketed by $bracket - ";
    say display ^$upto .grep: { .starts-with($bracket) && .ends-with($bracket) && .is-prime }
}

sub display ($list, :$cols = 10, :$fmt = '%6d', :$title = "{+$list} matching:\n" )   {
    cache $list;
    $title ~ $list.batch($cols)».fmt($fmt).join: "\n"
}
```

#### Output:
```
Primes up to 10000 bracketed by 1 - 39 matching:
    11    101    131    151    181    191   1021   1031   1051   1061
  1091   1151   1171   1181   1201   1231   1291   1301   1321   1361
  1381   1451   1471   1481   1511   1531   1571   1601   1621   1721
  1741   1801   1811   1831   1861   1871   1901   1931   1951

Primes up to 10000 bracketed by 3 - 33 matching:
     3    313    353    373    383   3023   3083   3163   3203   3253
  3313   3323   3343   3373   3413   3433   3463   3533   3583   3593
  3613   3623   3643   3673   3733   3793   3803   3823   3833   3853
  3863   3923   3943

Primes up to 10000 bracketed by 5 - 1 matching:
     5

Primes up to 10000 bracketed by 7 - 35 matching:
     7    727    757    787    797   7027   7057   7127   7177   7187
  7207   7237   7247   7297   7307   7417   7457   7477   7487   7507
  7517   7537   7547   7577   7607   7687   7717   7727   7757   7817
  7867   7877   7907   7927   7937

Primes up to 10000 bracketed by 9 - 29 matching:
   919    929   9029   9049   9059   9109   9199   9209   9239   9319
  9349   9419   9439   9479   9539   9619   9629   9649   9679   9689
  9719   9739   9749   9769   9829   9839   9859   9929   9949
```
