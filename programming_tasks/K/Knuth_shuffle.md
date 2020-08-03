[1]: https://rosettacode.org/wiki/Knuth_shuffle

# [Knuth shuffle][1]

```raku
sub shuffle (@a is copy) {
    for 1 ..^ @a -> $n {
        my $k = (0 .. $n).pick;
        $k == $n or @a[$k, $n] = @a[$n, $k];
    }
    return @a;
}
```


The shuffle is also built into the pick method on lists when you pass it a "whatever" for the number to pick:

```raku
my @deck = @cards.pick(*);
```