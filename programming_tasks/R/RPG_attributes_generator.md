[1]: https://rosettacode.org/wiki/RPG_attributes_generator

# [RPG attributes generator][1]



```perl
my ( $min_sum, $hero_attr_min, $hero_count_min ) = 75, 15, 2;
my @attr-names = <Str Int Wis Dex Con Cha>;
 
sub heroic { + @^a.grep: * >= $hero_attr_min }
 
my @attr;
repeat until @attr.sum     >= $min_sum
         and heroic(@attr) >= $hero_count_min {
 
    @attr = @attr-names.map: { (1..6).roll(4).sort(+*).skip(1).sum };
}
 
say @attr-names Z=> @attr;
say "Sum: {@attr.sum}, with {heroic(@attr)} attributes >= $hero_attr_min";
```

#### Output:
```
(Str => 15 Int => 16 Wis => 13 Dex => 11 Con => 15 Cha => 6)
Sum: 76, with 3 attributes >= 15
```
