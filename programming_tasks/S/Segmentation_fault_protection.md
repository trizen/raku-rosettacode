[1]: https://rosettacode.org/wiki/Segmentation_fault_protection

# [Segmentation fault protection][1]

Barring bugs in the compiler implementation, it should be impossible to generate a seqfault in standard Raku code. Memory is automatically managed, allocated and garbage collected by the virtual machine. Arrays are automatically managed, with storage allocation autovivified on demand. Uninitialized variables default to a Nil value, which decomposes to the base type of the variable. (If a base type was not specified, it could be Any type.)

```perl
my @array = [1, 2, 3, 4, 5, 6];
my $var = @array[500];
 
say $var;
```

#### Output:
```
(Any)
```


If you invoke the Nativecall interface and call out into C code or libraries however, all such protections are forfeit. With great power comes great responsibility.
