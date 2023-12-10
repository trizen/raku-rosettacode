[1]: https://rosettacode.org/wiki/Unique_characters_in_each_string

# [Unique characters in each string][1]

```perl
my $strings = <1a3c52debeffd 2b6178c97a938stf 3ycxdb1fgxa2yz>;

put sort keys [∩] $strings.map: *.comb.Bag.grep: *.value == 1
```

#### Output:
```
1 2 3 a b c
```
