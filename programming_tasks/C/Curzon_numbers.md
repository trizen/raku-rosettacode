[1]: https://rosettacode.org/wiki/Curzon_numbers

# [Curzon numbers][1]

```perl
sub curzon ($base) { lazy (1..∞).hyper.map: { $_ if (exp($_, $base) + 1) %% ($base × $_ + 1) } };
 
for <2 4 6 8 10> {
    my $curzon = .&curzon;
    say "\nFirst 50 Curzon numbers using a base of $_:\n" ~
      $curzon[^50].batch(25)».fmt("%4s").join("\n") ~
      "\nOne thousandth: " ~ $curzon[999]
}
```

#### Output:
```
First 50 Curzon numbers using a base of 2:
   1    2    5    6    9   14   18   21   26   29   30   33   41   50   53   54   65   69   74   78   81   86   89   90   98
 105  113  114  125  134  138  141  146  153  158  165  173  174  186  189  194  198  209  210  221  230  233  245  249  254
One thousandth: 8646

First 50 Curzon numbers using a base of 4:
   1    3    7    9   13   15   25   27   37   39   43   45   49   57   67   69   73   79   87   93   97   99  105  115  127
 135  139  153  163  165  169  175  177  183  189  193  199  205  207  213  219  235  249  253  255  265  267  273  277  279
One thousandth: 9375

First 50 Curzon numbers using a base of 6:
   1    6   30   58   70   73   90  101  105  121  125  146  153  166  170  181  182  185  210  233  241  242  266  282  290
 322  373  381  385  390  397  441  445  446  450  453  530  557  562  585  593  601  602  605  606  621  646  653  670  685
One thousandth: 20717

First 50 Curzon numbers using a base of 8:
   1   14   35   44   72   74   77  129  131  137  144  149  150  185  200  219  236  266  284  285  299  309  336  357  381
 386  390  392  402  414  420  441  455  459  470  479  500  519  527  536  557  582  600  602  617  639  654  674  696  735
One thousandth: 22176

First 50 Curzon numbers using a base of 10:
   1    9   10   25  106  145  190  193  238  253  306  318  349  385  402  462  486  526  610  649  658  678  733  762  810
 990  994 1033 1077 1125 1126 1141 1149 1230 1405 1422 1441 1485 1509 1510 1513 1606 1614 1630 1665 1681 1690 1702 1785 1837
One thousandth: 46845
```
