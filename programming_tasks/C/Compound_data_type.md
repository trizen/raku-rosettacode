[1]: http://rosettacode.org/wiki/Compound_data_type

# [Compound data type][1]

```perl6
my @point = 3, 8;
```
```perl6
my %point = x => 3, y => 8;
```
```perl6
class Point { has $.x is rw; has $.y is rw; }
my Point $point .= new(x => 3, y => 8);
```
```perl6
my $s1 = set <a b c d>; # order is not preserved
my $s2 = set <c d e f>;
say $s1 (&) $s2; # OUTPUT«set(c, e)»
say $s1 ∩ $s2; # we also do Unicode
```