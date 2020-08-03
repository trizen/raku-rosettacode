[1]: https://rosettacode.org/wiki/Enforced_immutability

# [Enforced immutability][1]

You can create constants in Perl 6 with constant:

```raku
constant $pi = 3.14159;
constant $msg = "Hello World";
 
constant @arr = (1, 2, 3, 4, 5);
```


Immutability is abstract enough that you can define an infinite constant lazily:

```raku
constant fibonacci = 0, 1, *+* ... *;
```


Variables are considered mutable by default, but may be marked as readonly after initialization:

```raku
my $pi := 3 + rand;
```


Unlike variables, formal parameters are considered readonly by default even if bound to a mutable container.

```raku
sub sum (Num $x, Num $y) {
	$x += $y;  # ERROR
}
 
# Explicitly ask for pass-by-reference semantics
sub addto (Num $x is rw, Num $y) {
    $x += $y;  # ok, propagated back to caller
}
 
# Explicitly ask for pass-by-value semantics
sub sum (Num $x is copy, Num $y) {
    $x += $y;  # ok, but NOT propagated back to caller
    $x;
}
```


A number of built-in types are considered immutable value types, including:


#### Output:
```
Str         Perl string (finite sequence of Unicode characters)
Int         Perl integer (allows Inf/NaN, arbitrary precision, etc.)
Num         Perl number (approximate Real, generally via floating point)
Rat         Perl rational (exact Real, limited denominator)
FatRat      Perl rational (unlimited precision in both parts)
Complex     Perl complex number
Bool        Perl boolean
Exception   Perl exception
Block       Executable objects that have lexical scopes
Seq         A list of values (can be generated lazily)
Range       A pair of Ordered endpoints
Set         Unordered collection of values that allows no duplicates
Bag         Unordered collection of values that allows duplicates
Enum        An immutable Pair
Map         A mapping of Enums with no duplicate keys
Signature   Function parameters (left-hand side of a binding)
Capture     Function call arguments (right-hand side of a binding)
Blob        An undifferentiated mass of ints, an immutable Buf
Instant     A point on the continuous atomic timeline
Duration    The difference between two Instants
```


These values, though objects, can't mutate; they may only be "changed" by modifying a mutable container holding one of them to hold a different value instead. (In the abstract, that is. In the interests of efficiency, a string or list implementation would be allowed to cheat as long as it doesn't get caught cheating.) Some of these types have corresponding "unboxed" native representations, where the container itself must carry the type information since the value can't. In this case, it's still the container that might be
considered mutable as an lvalue location, not the value stored in that location.



By default, object attributes are not modifiable from outside a class, though this is usually viewed more as encapsulation than as mutability control.