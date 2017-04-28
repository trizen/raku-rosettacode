[1]: http://rosettacode.org/wiki/Kronecker_product

# [Kronecker product][1]

```perl
sub kronecker_product ( @a, @b ) {
    return (@a X @b).map: { .[0].list X* .[1].list };
}
 
.say for kronecker_product([ <1 2>, <3 4> ],
                           [ <0 5>, <6 7> ]);
say '';
.say for kronecker_product([ <0 1 0>,   <1 1 1>,   <0 1 0>  ],
                           [ <1 1 1 1>, <1 0 0 1>, <1 1 1 1>]);
 
```

#### Output:
```
(0 5 0 10)
(6 7 12 14)
(0 15 0 20)
(18 21 24 28)

(0 0 0 0 1 1 1 1 0 0 0 0)
(0 0 0 0 1 0 0 1 0 0 0 0)
(0 0 0 0 1 1 1 1 0 0 0 0)
(1 1 1 1 1 1 1 1 1 1 1 1)
(1 0 0 1 1 0 0 1 1 0 0 1)
(1 1 1 1 1 1 1 1 1 1 1 1)
(0 0 0 0 1 1 1 1 0 0 0 0)
(0 0 0 0 1 0 0 1 0 0 0 0)
(0 0 0 0 1 1 1 1 0 0 0 0)
```