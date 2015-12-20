[1]: http://rosettacode.org/wiki/Abstract_type

# [Abstract type][1]

Perl 6 supports roles, which are a bit like interfaces, but unlike interfaces in Java they can also contain some implementation.

```perl
 
use v6;
 
role A {
    # must be filled in by the class it is composed into
    method abstract() { ... };
 
    # can be overridden in the class, but that's not mandatory
    method concrete() { say '# 42' };
}
 
class SomeClass does A {
    method abstract() {
        say "# made concrete in class"
    }
}
 
my $obj = SomeClass.new;
$obj.abstract();
$obj.concrete();
 
# output:
# made concrete in class
# 42
 
```