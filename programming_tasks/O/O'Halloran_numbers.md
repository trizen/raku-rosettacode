[1]: https://rosettacode.org/wiki/O%27Halloran_numbers

# [O&#039;Halloran numbers][1]

```perl
my @Area;

my $threshold = 1000; # a little overboard to make sure we get them all

for 1..$threshold -> $x {
    for 1..$x -> $y {
        last if $x × $y > $threshold;
        for 1..$y -> $z {
           push @Area[my $area = ($x × $y + $y × $z + $z × $x) × 2], "$x,$y,$z";
           last if $area > $threshold;
        }
    }
}

say "Even integer surface areas NOT achievable by any regular, integer dimensioned cuboid:\n" ~
   @Area[^$threshold].kv.grep( { $^key > 6 and $key %% 2 and !$^value } )»[0];
```

#### Output:
```
Even integer surface areas NOT achievable by any regular, integer dimensioned cuboid:
8 12 20 36 44 60 84 116 140 156 204 260 380 420 660 924
```
