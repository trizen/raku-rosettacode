[1]: http://rosettacode.org/wiki/Compile-time_calculation

# [Compile-time calculation][1]

```perl6
constant $tenfact = [*] 2..10; 
say $tenfact;
```


Like Perl 5, we also have a BEGIN block, but it also works to introduce a blockless statement,
the value of which will be stored up to be used in the surrounding expression at run time:

```perl6
 say(BEGIN [*] 2..10);
```