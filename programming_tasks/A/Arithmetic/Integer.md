[1]: http://rosettacode.org/wiki/Arithmetic/Integer

# [Arithmetic/Integer][1]

```perl6
my Int $a = get.floor;
my Int $b = get.floor;
 
say 'sum:              ', $a + $b;
say 'difference:       ', $a - $b;
say 'product:          ', $a * $b;
say 'integer quotient: ', $a div $b;
say 'remainder:        ', $a % $b;
say 'exponentiation:   ', $a**$b;
```


Note that `div` doesn't always do integer division; it performs the operation "most appropriate to the
operand types". [Synopsis 3](http://perlcabal.org/syn/S03.html#line\_729) guarantees that `div` "on built-in integer types is equivalent to taking the floor of a real division". If you want integer division with other types, say `floor($a/$b)`.