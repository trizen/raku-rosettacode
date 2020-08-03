[1]: https://rosettacode.org/wiki/Semordnilap

# [Semordnilap][1]

```perl
my $words = set slurp("unixdict.txt").lines;
 
my @sems = gather for $words.flat -> $word {
    my $drow = $word.key.flip;
    take $drow if $drow ∈ $words and $drow lt $word;
}
 
say $_ ~ ' ' ~ $_.flip for @sems.pick(5);
```

#### Output:
```
abut tuba
avid diva
bard drab
loot tool
part trap
```