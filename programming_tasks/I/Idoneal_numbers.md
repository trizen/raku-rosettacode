[1]: https://rosettacode.org/wiki/Idoneal_numbers

# [Idoneal numbers][1]

First 60 in less than 1/2 second. The remaining 5 take another ~5 seconds.

```perl
sub is-idoneal ($n) {
    my $idoneal = True;
    I: for 1 .. $n -> $a {
        for $a ^.. $n -> $b {
            last if $a × $b + $a + $b > $n; # short circuit
            for $b ^.. $n -> $c {
                $idoneal = False and last I if (my $sum = $a × $b + $b × $c + $c × $a) == $n;
                last if $sum > $n; # short circuit
            }
        }
    }
    $idoneal
}

$_».fmt("%4d").put for (1..1850).hyper(:32batch).grep( &is-idoneal ).batch(10)
```

#### Output:
```
   1    2    3    4    5    6    7    8    9   10
  12   13   15   16   18   21   22   24   25   28
  30   33   37   40   42   45   48   57   58   60
  70   72   78   85   88   93  102  105  112  120
 130  133  165  168  177  190  210  232  240  253
 273  280  312  330  345  357  385  408  462  520
 760  840 1320 1365 1848
```
