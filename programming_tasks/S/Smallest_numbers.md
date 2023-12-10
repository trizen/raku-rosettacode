[1]: https://rosettacode.org/wiki/Smallest_numbers

# [Smallest numbers][1]

```perl
sub smallest ( $n ) {
    state  @powers = '', |map { $_ ** $_ }, 1 .. *;

    return @powers.first: :k, *.contains($n);
}

.say for (^51).map(&smallest).batch(10)».fmt('%2d');
```

#### Output:
```
( 9  1  3  5  2  4  4  3  7  9)
(10 11  5 19 22 26  8 17 16 19)
( 9  8 13  7 17  4 17  3 11 18)
(13  5 23 17 18  7 17 15  9 18)
(16 17  9  7 12 28  6 23  9 24)
(23)
```
