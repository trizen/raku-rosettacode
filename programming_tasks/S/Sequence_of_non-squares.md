[1]: https://rosettacode.org/wiki/Sequence_of_non-squares

# [Sequence of non-squares][1]

```perl
sub nth-term (Int $n) { $n + round sqrt $n }
 
# Print the first 22 values of the sequence
say (nth-term $_ for 1 .. 22);
 
# Check that the first million values of the sequence are indeed non-square
for 1 .. 1_000_000 -> $i {
    say "Oops, nth-term($i) is square!" if (sqrt nth-term $i) %% 1;
}
```

#### Output:
```
(2 3 5 6 7 8 10 11 12 13 14 15 17 18 19 20 21 22 23 24 26 27)
```