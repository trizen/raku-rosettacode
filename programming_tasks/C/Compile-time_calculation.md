[1]: https://rosettacode.org/wiki/Compile-time_calculation

# [Compile-time calculation][1]



```perl
constant $tenfact = [*] 2..10; 
say $tenfact;
```


Like Perl 5, we also have a BEGIN block, but it also works to introduce a blockless statement,
the value of which will be stored up to be used as an expression at run time:

```perl
 say BEGIN [*] 2..10;
```
