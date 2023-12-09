[1]: https://rosettacode.org/wiki/Product_of_min_and_max_prime_factors

# [Product of min and max prime factors][1]

```perl
use Prime::Factor;
put "Product of smallest and greatest prime factors of n for 1 to 100:\n" ~
  (1..100).map({ 1 max .max × .min given cache .&prime-factors })».fmt("%4d").batch(10).join: "\n";
```

#### Output:
```
Product of smallest and greatest prime factors of n for 1 to 100:
   1    4    9    4   25    6   49    4    9   10
 121    6  169   14   15    4  289    6  361   10
  21   22  529    6   25   26    9   14  841   10
 961    4   33   34   35    6 1369   38   39   10
1681   14 1849   22   15   46 2209    6   49   10
  51   26 2809    6   55   14   57   58 3481   10
3721   62   21    4   65   22 4489   34   69   14
5041    6 5329   74   15   38   77   26 6241   10
   9   82 6889   14   85   86   87   22 7921   10
  91   46   93   94   95    6 9409   14   33   10
```
