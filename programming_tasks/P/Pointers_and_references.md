[1]: http://rosettacode.org/wiki/Pointers_and_references

# [Pointers and references][1]

In Perl 6 all non-native values are boxed and accessed via implicit references. (This is like Java or Python, but unlike C or Perl 5, which use explicit referencing and dereferencing.) Variables are references to containers that can contain references to other values. Basic binding (aliasing) of references to names is supported via the <tt>:=</tt> operator, while assignment to mutable containers implies a dereference from the name to the container, followed by copying of values rather than by duplicating pointers. (Assignment of a bare object reference copies the reference as if it were a value, but the receiving container automatically dereferences as necessary, so to all appearances you are putting the object itself into the destination rather than its reference, and we just think the object can be in more than one place at the same time.)

```perl
my $foo = 42;    # place a reference to 42 in $foo's item container
$foo++;          # deref $foo name, then increment the container's contents to 43
$foo.say;        # deref $foo name, then $foo's container, and call a method on 43.
 
$foo := 42;      # bind a direct ref to 42
$foo++;          # ERROR, cannot modify immutable value
 
my @bar = 1,2,3; # deref @bar name to array container, then set its values
@bar»++;         # deref @bar name to array container, then increment each value with a hyper
@bar.say;        # deref @bar name to array container, then call say on that, giving 2 3 4
 
@bar := (1,2,3); # bind name directly to a List
@bar»++;         # ERROR, parcels are not mutable
```


References to hashes and functions work more like arrays, insofar as a method call acts directly on the container, not on what the container contains. That is, they don't do the extra dereference implied by calling a method on a scalar variable.



To the first approximation, Perl 6 programmers do not think about references much; since everything is a reference, and value semantics are emulated by assign and other mutating operators, the ubiquitous references are largely transparent to the Perl 6 programmer most of the time.