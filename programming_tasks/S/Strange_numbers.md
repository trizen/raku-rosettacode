[1]: https://rosettacode.org/wiki/Strange_numbers

# [Strange numbers][1]

```perl
sub infix:<′>(\x,\y) { abs(x - y) ∈ (2, 3, 5, 7) }

my ($low,$high) = 100, 500;
my @strange.push: $_ if so all .comb.rotor(2 => -1).map: { .[0] ′ .[1] } for $low ^..^ $high;

say "Between $low and $high there are {+@strange} strange numbers:\n";
print @strange.batch(20)».fmt("%4d").join: "\n";
```

#### Output:
```
Between 100 and 500 there are 87 strange numbers:

 130  131  135  136  138  141  142  146  147  149  161  163  164  168  169  181  183  185  186  202
 203  205  207  241  242  246  247  249  250  252  253  257  258  270  272  274  275  279  292  294
 296  297  302  303  305  307  313  314  316  318  350  352  353  357  358  361  363  364  368  369
 381  383  385  386  413  414  416  418  420  424  425  427  429  461  463  464  468  469  470  472
 474  475  479  492  494  496  497
```
