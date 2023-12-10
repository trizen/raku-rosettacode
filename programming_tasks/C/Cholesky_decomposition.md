[1]: https://rosettacode.org/wiki/Cholesky_decomposition

# [Cholesky decomposition][1]



```perl
sub cholesky(@A) {
    my @L = @A »×» 0;
    for ^@A -> \i {
        for 0..i -> \j {
            @L[i;j] = (i == j ?? &sqrt !! 1/@L[j;j] × * )\      # select function
                      (@A[i;j] - [+] (@L[i;*] Z× @L[j;*])[^j])  # provide value
        }
    }
    @L
}

.fmt('%3d').say for cholesky [
    [25],
    [15, 18],
    [-5,  0, 11],
];

say '';

.fmt('%6.3f').say for cholesky [
    [18, 22,  54,  42],       
    [22, 70,  86,  62],
    [54, 86, 174, 134],       
    [42, 62, 134, 106],
];
```

#### Output:
```
  5
  3   3
 -1   1   3

 4.243  0.000  0.000  0.000
 5.185  6.566  0.000  0.000
12.728  3.046  1.650  0.000
 9.899  1.625  1.850  1.393
```
