[1]: https://rosettacode.org/wiki/Collections

# [Collections][1]

Perl 6 has both mutable and immutable containers of various sorts. Here are some of the most common ones:



### Mutable

```perl
# Array
my @array = 1,2,3;
@array.push: 4,5,6;
 
# Hash
my %hash = 'a' => 1, 'b' => 2;
%hash<c d> = 3,4;
%hash.push: 'e' => 5, 'f' => 6;
 
# SetHash
my $s = SetHash.new: <a b c>;
$s ∪= <d e f>;
 
# BagHash
my $b = BagHash.new: <b a k l a v a>;
$b ⊎= <a b c>;
```


### Immutable

```perl
# List
my @list := 1,2,3;
my @newlist := |@list, 4,5,6; # |@list will slip @list into the surrounding list instead of creating a list of lists
 
# Set
my $set = set <a b c>;
my $newset = $set ∪ <d e f>;
 
# Bag
my $bag = bag <b a k l a v a>;
my $newbag = $bag ⊎ <b e e f>;
```


### Pair list (cons list)

```perl
my $tail = d => e => f => Nil;
my $new = a => b => c => $tail;
```


### P6opaque object (immutable in structure)

```perl
class Something { has $.foo; has $.bar };
my $obj = Something.new: foo => 1, bar => 2;
my $newobj = $obj but role { has $.baz = 3 } # anonymous mixin
```