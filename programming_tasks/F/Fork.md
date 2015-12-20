[1]: http://rosettacode.org/wiki/Fork

# [Fork][1]

```perl
use NativeCall;
sub fork() returns Int is native { ... }
 
if fork() -> $pid {
    print "I am the proud parent of $pid.\n";
}
else {
    print "I am a child.  Have you seen my mommy?\n";
}
```

#### Output:
```
I am the proud parent of 17691.
I am a child.  Have you seen my mommy?
```