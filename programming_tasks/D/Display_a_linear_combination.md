[1]: https://rosettacode.org/wiki/Display_a_linear_combination

# [Display a linear combination][1]

```perl
sub linear-combination(@coeff) {
    (@coeff Z=> map { "e($_)" }, 1 .. *)
    .grep(+*.key)
    .map({ .key ~ '*' ~ .value })
    .join(' + ')
    .subst('+ -', '- ', :g)
    .subst(/<|w>1\*/, '', :g)
        || '0'
}
 
say linear-combination($_) for 
[1, 2, 3],
[0, 1, 2, 3],
[1, 0, 3, 4],
[1, 2, 0],
[0, 0, 0],
[0],
[1, 1, 1],
[-1, -1, -1],
[-1, -2, 0, -3],
[-1 ]
;
```

#### Output:
```
e(1) + 2*e(2) + 3*e(3)
e(2) + 2*e(3) + 3*e(4)
e(1) + 3*e(3) + 4*e(4)
e(1) + 2*e(2)
0
0
e(1) + e(2) + e(3)
-e(1) - e(2) - e(3)
-e(1) - 2*e(2) - 3*e(4)
-e(1)
```