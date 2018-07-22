[1]: https://rosettacode.org/wiki/Compound_data_type

# [Compound data type][1]

### Array

```perl
my @point = 3, 8;
 
my Int @point = 3, 8; # or constrain to integer elements
```


### Hash

```perl
my %point = x => 3, y => 8;
 
my Int %point = x => 3, y => 8; # or constrain the hash to have integer values
```


### Class instance

```perl
class Point { has $.x is rw; has $.y is rw; }
my Point $point .= new(x => 3, y => 8);
```


### [Set](http://design.perl6.org/S32/Containers.html#Set)

```perl
my $s1 = set <a b c d>; # order is not preserved
my $s2 = set <c d e f>;
say $s1 (&) $s2; # OUTPUT«set(c, e)»
say $s1 ∩ $s2; # we also do Unicode
```