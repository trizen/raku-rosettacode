[1]: https://rosettacode.org/wiki/Sum_of_elements_below_main_diagonal_of_matrix

# [Sum of elements below main diagonal of matrix][1]

```perl
sub lower-triangle-sum (@matrix) { sum flat (1..@matrix).map( { @matrix[^$_]»[^($_-1)] } )»[*-1] }
 
say lower-triangle-sum 
[
    [  1,  3,  7,  8, 10 ],
    [  2,  4, 16, 14,  4 ],
    [  3,  1,  9, 18, 11 ],
    [ 12, 14, 17, 18, 20 ],
    [  7,  1,  3,  9,  5 ]
];
```

#### Output:
```
69
```
