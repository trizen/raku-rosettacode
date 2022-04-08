[1]: https://rosettacode.org/wiki/Special_pythagorean_triplet

# [Special pythagorean triplet][1]

```perl
hyper for 1..998 -> $a {
    my $a2 = $a²;
    for $a + 1 .. 999 -> $b {
        my $c = 1000 - $a - $b;
        last if $c < $b;
        say "$a² + $b² = $c²\n$a  + $b  + $c = {$a+$b+$c}\n$a  × $b  × $c = {$a×$b×$c}"
            and exit if $a2 + $b² == $c²
    }
}
```

#### Output:
```
200² + 375² = 425²
200  + 375  + 425 = 1000
200  × 375  × 425 = 31875000
```
