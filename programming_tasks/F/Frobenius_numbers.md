[1]: https://rosettacode.org/wiki/Frobenius_numbers

# [Frobenius numbers][1]

```perl
say "{+$_} matching numbers\n{.batch(10)».fmt('%4d').join: "\n"}\n"
    given (^1000).grep( *.is-prime ).rotor(2 => -1)
    .map( { (.[0] * .[1] - .[0] - .[1]) } ).grep(* < 10000);
```

#### Output:
```
25 matching numbers
   1    7   23   59  119  191  287  395  615  839
1079 1439 1679 1931 2391 3015 3479 3959 4619 5039
5615 6395 7215 8447 9599
```
