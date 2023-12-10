[1]: https://rosettacode.org/wiki/Best_shuffle

# [Best shuffle][1]



```perl
sub best-shuffle(Str $orig) {
    my @s = $orig.comb;
    my @t = @s.pick(*);

    for flat ^@s X ^@s -> \i,\j {
        if i != j and @t[i] ne @s[j] and @t[j] ne @s[i] {
            @t[i,j] = @t[j,i] and last
        }
    }

    my $count = 0;
    for @t.kv -> $k,$v {
        ++$count if $v eq @s[$k]
    }

    @t.join, $count;
}

printf "%s, %s, (%d)\n", $_, best-shuffle $_  for <abracadabra seesaw elk grrrrrr up a>;
```

#### Output:
```
abracadabra, raacarabadb, (0)
seesaw, wssaee, (0)
elk, lke, (0)
grrrrrr, rrrgrrr, (5)
up, pu, (0)
a, a, (1)
```
