[1]: http://rosettacode.org/wiki/Hash_from_two_arrays

# [Hash from two arrays][1]

```perl
my @keys = <a b c d e>;
my @vals = ^5;
my %hash = flat @keys Z @vals;
```


or using the "zipwith" metaoperaotr on the <tt>=&gt;</tt> pair composer:

```perl
my @v = <a b c d e>;
my %hash = @v Z=> @v.keys;
```




Alternatively:

```perl
my %hash;
%hash{@keys} = @vals;
```


To create an anonymous hash value:

```perl
%( <a b c d e> Z=> ^5 )
```


All of these zip forms trim the result to the length of the shorter of their two input lists. If you wish to enforce equal lengths, you can use a strict hyperoperator instead:

```perl
{ @keys »=>« @values }
```


This will fail if the lists differ in length.