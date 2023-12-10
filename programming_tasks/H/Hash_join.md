[1]: https://rosettacode.org/wiki/Hash_join

# [Hash join][1]





The `.classify` method returns a multimap represented as a `Hash` whose values are `Array`s.

```perl
sub hash-join(@a, &a, @b, &b) {
    my %hash := @b.classify(&b);
    
    @a.map: -> $a {
        |(%hash{$a.&a} // next).map: -> $b { [$a, $b] }
    }
}

my @A =
    [27, "Jonah"],
    [18, "Alan"],
    [28, "Glory"],
    [18, "Popeye"],
    [28, "Alan"],
;

my @B =
    ["Jonah", "Whales"],
    ["Jonah", "Spiders"],
    ["Alan", "Ghosts"],
    ["Alan", "Zombies"],
    ["Glory", "Buffy"],
;

.say for hash-join @A, *[1], @B, *[0];
```

#### Output:
```
[[27 Jonah] [Jonah Whales]]
[[27 Jonah] [Jonah Spiders]]
[[18 Alan] [Alan Ghosts]]
[[18 Alan] [Alan Zombies]]
[[28 Glory] [Glory Buffy]]
[[28 Alan] [Alan Ghosts]]
[[28 Alan] [Alan Zombies]]
```
