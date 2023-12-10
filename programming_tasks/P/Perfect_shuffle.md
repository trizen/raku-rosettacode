[1]: https://rosettacode.org/wiki/Perfect_shuffle

# [Perfect shuffle][1]



```perl
for 8, 24, 52, 100, 1020, 1024, 10000 -> $size {
    my ($n, @deck) = 1, |^$size;
    $n++ until [<] @deck = flat [Z] @deck.rotor: @deck/2;
    printf "%5d cards: %4d\n", $size, $n;
}
```

#### Output:
```
    8 cards:    3
   24 cards:   11
   52 cards:    8
  100 cards:   30
 1020 cards: 1018
 1024 cards:   10
10000 cards:  300
```
