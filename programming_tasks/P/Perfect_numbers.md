[1]: http://rosettacode.org/wiki/Perfect_numbers

# [Perfect numbers][1]

```perl
sub perf($n) { $n == [+] grep $n %% *, 1 .. $n div 2 }
```