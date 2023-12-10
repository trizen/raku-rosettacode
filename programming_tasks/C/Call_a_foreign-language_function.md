[1]: https://rosettacode.org/wiki/Call_a_foreign-language_function

# [Call a foreign-language function][1]



```perl
use NativeCall;

sub strdup(Str $s --> Pointer) is native {*}
sub puts(Pointer $p --> int32) is native {*}
sub free(Pointer $p --> int32) is native {*}

my $p = strdup("Success!");
say 'puts returns ', puts($p);
say 'free returns ', free($p);
```

#### Output:
```
Success!
puts returns 9
free returns 0
```
