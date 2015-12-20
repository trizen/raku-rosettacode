[1]: http://rosettacode.org/wiki/Decimal_floating_point_number_to_binary

# [Decimal floating point number to binary][1]

```perl6
given "23.34375"   { say "$_ => ", :10($_).base(2) }
given "1011.11101" { say "$_ => ", :2($_).base(10) }
```

#### Output:
```
23.34375 => 10111.01011
1011.11101 => 11.90625
```