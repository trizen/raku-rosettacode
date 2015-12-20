[1]: http://rosettacode.org/wiki/Call_a_foreign-language_function

# [Call a foreign-language function][1]

```perl
use NativeCall;
 
sub strdup(Str $s --> OpaquePointer) is native {*}
sub puts(OpaquePointer $p --> int) is native {*}
sub free(OpaquePointer $p --> int) is native {*}
 
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