[1]: https://rosettacode.org/wiki/Leonardo_numbers

# [Leonardo numbers][1]

```perl
sub 𝑳 ( $𝑳0 = 1, $𝑳1 = 1, $𝑳add = 1 ) { $𝑳0, $𝑳1, { $^n2 + $^n1 + $𝑳add } ... * }
 
# Part 1
say "The first 25 Leonardo numbers:";
put 𝑳()[^25];
 
# Part 2
say "\nThe first 25 numbers using 𝑳0 of 0, 𝑳1 of 1, and adder of 0:";
put 𝑳( 0, 1, 0 )[^25];
```

#### Output:
```
The first 25 Leonardo numbers:
1 1 3 5 9 15 25 41 67 109 177 287 465 753 1219 1973 3193 5167 8361 13529 21891 35421 57313 92735 150049

The first 25 numbers using 𝑳0 of 0, 𝑳1 of 1, and adder of 0:
0 1 1 2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597 2584 4181 6765 10946 17711 28657 46368
```