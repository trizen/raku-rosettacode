[1]: https://rosettacode.org/wiki/Unique_characters

# [Unique characters][1]

One has to wonder where the digits 0 through 9 come in the alphabet... 🤔 For that matter, What alphabet should they be in order of? Most of these entries seem to presuppose ASCII order but that isn't specified anywhere. What to do with characters outside of ASCII (or Latin-1)? Unicode ordinal order? Or maybe DUCET Unicode collation order? It's all very vague.

```perl
my @list = <133252abcdeeffd a6789798st yxcdfgxcyz>;

for @list, (@list, 'AАΑSäaoö٥🤔👨‍👩‍👧‍👧') {
    say "$_\nSemi-bogus \"Unicode natural sort\" order: ",
    .map( *.comb ).Bag.grep( *.value == 1 )».key.sort( { .unival, .NFKD[0], .fc } ).join,
    "\n        (DUCET) Unicode collation order: ",
    .map( *.comb ).Bag.grep( *.value == 1 )».key.collate.join, "\n";
}
```

#### Output:
```
133252abcdeeffd a6789798st yxcdfgxcyz
Semi-bogus "Unicode natural sort" order: 156bgstz
        (DUCET) Unicode collation order: 156bgstz

133252abcdeeffd a6789798st yxcdfgxcyz AАΑSäaoö٥🤔👨‍👩‍👧‍👧
Semi-bogus "Unicode natural sort" order: 15٥6ASäbgoöstzΑА👨‍👩‍👧‍👧🤔
        (DUCET) Unicode collation order: 👨‍👩‍👧‍👧🤔ä15٥6AbögosStzΑА
```
