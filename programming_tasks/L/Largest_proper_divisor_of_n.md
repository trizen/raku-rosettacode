[1]: https://rosettacode.org/wiki/Largest_proper_divisor_of_n

# [Largest proper divisor of n][1]

A little odd to special case a(1) == 1 as technically, 1 doesn't have any proper divisors... but it matches **[OEIS A032742](https://oeis.org/A032742)** so whatever.



Note: this example is ridiculously overpowered (and so, somewhat inefficient) for a range of 1 to 100, but will easily handle large numbers without modification.

```perl
use Prime::Factor;

for 0, 2**67 - 1 -> $add {
    my $start = now;

    my $range = $add + 1 .. $add + 100;

    say "GPD for {$range.min} through {$range.max}:";

    say ( $range.hyper(:14batch).map: {$_ == 1 ?? 1 !! $_ %% 2 ?? $_ / 2 !! .&proper-divisors.max} )\
        .batch(10)».fmt("%{$add.chars + 1}d").join: "\n";

    say (now - $start).fmt("%0.3f seconds\n");
}
```

#### Output:
```
GPD for 1 through 100:
 1  1  1  2  1  3  1  4  3  5
 1  6  1  7  5  8  1  9  1 10
 7 11  1 12  5 13  9 14  1 15
 1 16 11 17  7 18  1 19 13 20
 1 21  1 22 15 23  1 24  7 25
17 26  1 27 11 28 19 29  1 30
 1 31 21 32 13 33  1 34 23 35
 1 36  1 37 25 38 11 39  1 40
27 41  1 42 17 43 29 44  1 45
13 46 31 47 19 48  1 49 33 50
0.071 seconds

GPD for 147573952589676412928 through 147573952589676413027:
  73786976294838206464   49191317529892137643   73786976294838206465                      1   73786976294838206466   21081993227096630419   73786976294838206467   49191317529892137645   73786976294838206468    8680820740569200761
  73786976294838206469    5088756985850910791   73786976294838206470   49191317529892137647   73786976294838206471   13415813871788764813   73786976294838206472   29514790517935282589   73786976294838206473   49191317529892137649
  73786976294838206474    6416258808246800563   73786976294838206475     977310944302492801   73786976294838206476   49191317529892137651   73786976294838206477   29514790517935282591   73786976294838206478      87167130885810049
  73786976294838206479   49191317529892137653   73786976294838206480   21081993227096630423   73786976294838206481    7767050136298758577   73786976294838206482   49191317529892137655   73786976294838206483    2784414199805215339
  73786976294838206484   11351842506898185613   73786976294838206485   49191317529892137657   73786976294838206486     939961481462907089   73786976294838206487   29514790517935282595   73786976294838206488   49191317529892137659
  73786976294838206489      82581954443019817   73786976294838206490        147258377885867   73786976294838206491   49191317529892137661   73786976294838206492   29514790517935282597   73786976294838206493   13415813871788764817
  73786976294838206494   49191317529892137663   73786976294838206495                      1   73786976294838206496    2202596307308603179   73786976294838206497   49191317529892137665   73786976294838206498    5088756985850910793
  73786976294838206499                      1   73786976294838206500   49191317529892137667   73786976294838206501   21081993227096630429   73786976294838206502   29514790517935282601   73786976294838206503   49191317529892137669
  73786976294838206504   13415813871788764819   73786976294838206505     103270785577100359   73786976294838206506   49191317529892137671   73786976294838206507   29514790517935282603   73786976294838206508   21081993227096630431
  73786976294838206509   49191317529892137673   73786976294838206510   11351842506898185617   73786976294838206511                      1   73786976294838206512   49191317529892137675   73786976294838206513    1305964182209525779
0.440 seconds
```