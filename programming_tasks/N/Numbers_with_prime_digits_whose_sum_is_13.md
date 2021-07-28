[1]: https://rosettacode.org/wiki/Numbers_with_prime_digits_whose_sum_is_13

# [Numbers with prime digits whose sum is 13][1]

```perl
put join ', ', sort +*, unique flat
   < 2 2 2 2 2 3 3 3 5 5 7 >.combinations
   .grep( *.sum == 13 )
   .map( { .join => $_ } )
   .map: { .value.permutations».join }
```

#### Output:
```
337, 355, 373, 535, 553, 733, 2227, 2272, 2335, 2353, 2533, 2722, 3235, 3253, 3325, 3352, 3523, 3532, 5233, 5323, 5332, 7222, 22225, 22252, 22333, 22522, 23233, 23323, 23332, 25222, 32233, 32323, 32332, 33223, 33232, 33322, 52222, 222223, 222232, 222322, 223222, 232222, 322222
```
