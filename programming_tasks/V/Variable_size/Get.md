[1]: http://rosettacode.org/wiki/Variable_size/Get

# [Variable size/Get][1]

Perl 6 tries to avoid generic terms such as "size" and "length", instead providing methods that are expressed in terms of the units appropriate to the abstraction level.

```perl
# Textual strings are measured in characters (graphemes)
my $string = "abc";
 
# Arrays are measured in elements.
say $string.chars;     # 3
my @array = 1..5;
say @array.elems;      # 5
 
# Buffers may be viewed either as a byte-string or as an array of elements.
my $buffer = '#56997; means "four dragons".'.encode('utf8');
say $buffer.bytes;     # 26
say $buffer.elems;     # 26
```


Perl's Int type is arbitrary sized, and therefore the abstract size of the integer depends on how many bits are needed to represent it. While the internal representation is likely to be "chunky" by 32 or 64 bits, this is considered an implementation detail and is not revealed to the programmer.



Native types such as int64 or num32 are of fixed size; Perl 6 supports representational polymorphism of such types (and compositions of such types) through a pluggable meta-object protocol providing "knowhow" objects for the various representations. Currently the NativeHOW knowhow may be interrogated for the bit sizes of native types, but this is officially outside the purview of the language itself (so far).