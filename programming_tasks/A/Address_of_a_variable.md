[1]: http://rosettacode.org/wiki/Address_of_a_variable

# [Address of a variable][1]

```perl6
my $x; say $x.WHERE;
```

#### Output:
```
7857931379550584425
```


How you set the address of a variable (or any other object) is outside the purview of the Perl 6 language, but Perl 6 supports pluggable object representations, and any given representation scheme could conceivably allow an existing address to be treated as an object candidate where that makes sense. Memory-mapped structs are not unreasonable and are likely to be supported on VMs that allow it.