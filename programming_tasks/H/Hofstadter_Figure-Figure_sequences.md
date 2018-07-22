[1]: https://rosettacode.org/wiki/Hofstadter_Figure-Figure_sequences

# [Hofstadter Figure-Figure sequences][1]

```perl
my %r = 1 => 1;
my %s = 1 => 2;
 
sub ffr ($n) { %r{$n} //= ffr($n - 1) + ffs($n - 1) }
sub ffs ($n) { %s{$n} //= (grep none(map &ffr, 1..$n), max(%s.values)+1..*)[0] }
 
my @ffr = map &ffr, 1..*;
my @ffs = map &ffs, 1..*;
 
say @ffr[^10];
say "Rawks!" if 1...1000 eqv sort |@ffr[^40], |@ffs[^960];
```


Output:


#### Output:
```
1 3 7 12 18 26 35 45 56 69
Rawks!
```