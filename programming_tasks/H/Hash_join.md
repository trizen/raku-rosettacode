[1]: http://rosettacode.org/wiki/Hash_join

# [Hash join][1]

```perl6
my @A = [1, "Jonah"],
        [2, "Alan"],
        [3, "Glory"],
        [4, "Popeye"];
 
my @B = ["Jonah", "Whales"],
        ["Jonah", "Spiders"],
        ["Alan", "Ghosts"],
        ["Alan", "Zombies"],
        ["Glory", "Buffy"];
 
sub hash-join(@a, &a, @b, &b) {
    my %hash{Any};
    %hash{.&a} = $_ for @a;
    ([%hash{.&b} // next, $_] for @b);
}
 
.perl.say for hash-join @A, *.[1], @B, *.[0];
```

#### Output:
```
[[1, "Jonah"], ["Jonah", "Whales"]]
[[1, "Jonah"], ["Jonah", "Spiders"]]
[[2, "Alan"], ["Alan", "Ghosts"]]
[[2, "Alan"], ["Alan", "Zombies"]]
[[3, "Glory"], ["Glory", "Buffy"]]
```