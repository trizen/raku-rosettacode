[1]: https://rosettacode.org/wiki/Factors_of_an_integer

# [Factors of an integer][1]



```perl
sub factors (Int $n) { (1..$n).grep($n %% *) }
```
