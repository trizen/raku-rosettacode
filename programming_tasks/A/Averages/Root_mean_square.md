[1]: https://rosettacode.org/wiki/Averages/Root_mean_square

# [Averages/Root mean square][1]

```perl
sub rms(*@nums) { sqrt [+](@nums X** 2) / @nums }
 
say rms 1..10;
```


Here's a slightly more concise version, albeit arguably less readable:

```perl
sub rms { sqrt @_ R/ [+] @_ X** 2 }
```