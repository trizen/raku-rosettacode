[1]: https://rosettacode.org/wiki/Associative_array/Creation

# [Associative array/Creation][1]





The fatarrow, `=>`, is no longer just a quoting comma; it now constructs a `Pair` object. But you can still define a hash with an ordinary list of even length.

```perl
my %h1 = key1 => 'val1', 'key-2' => 2, three => -238.83, 4 => 'val3';
my %h2 = 'key1', 'val1', 'key-2', 2, 'three', -238.83, 4, 'val3';

# Creating a hash from two lists using a metaoperator.

my @a = 1..5;
my @b = 'a'..'e';
my %h = @a Z=> @b;

# Hash elements and hash slices now use the same sigil as the whole hash. This is construed as a feature.
# Curly braces no longer auto-quote, but Raku's qw (shortcut < ... >) now auto-subscripts.

say %h1{'key1'};
say %h1<key1>;
%h1<key1> = 'val1';
%h1<key1 three> = 'val1', -238.83;

# Special syntax is no longer necessary to access a hash stored in a scalar.

my $h = {key1 => 'val1', 'key-2' => 2, three => -238.83, 4 => 'val3'};
say $h<key1>;

# Keys are of type Str or Int by default. The type of the key can be provided.

my %hash{Any}; # same as %hash{*}
class C {};
my %cash{C};
%cash{C.new} = 1;
```
