[1]: https://rosettacode.org/wiki/Pseudo-random_numbers/Middle-square_method

# [Pseudo-random numbers/Middle-square method][1]

```perl
sub msq {
    state $seed = 675248;
    $seed = $seed² div 1000 mod 1000000;
}

say msq() xx 5;
```

#### Output:
```
(959861 333139 981593 524817 432883)
```
