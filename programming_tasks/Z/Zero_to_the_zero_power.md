[1]: https://rosettacode.org/wiki/Zero_to_the_zero_power

# [Zero to the zero power][1]



```perl
say '    type         n      n**n  exp(n,n)';
say '--------  --------  --------  --------';

for 0, 0.0, FatRat.new(0), 0e0, 0+0i {
    printf "%8s  %8s  %8s  %8s\n", .^name, $_, $_**$_, exp($_,$_);
}
```

#### Output:
```
    type         n      n**n  exp(n,n)
--------  --------  --------  --------
     Int         0         1         1
     Rat         0         1         1
  FatRat         0         1         1
     Num         0         1         1
 Complex      0+0i      1+0i      1+0i
```
