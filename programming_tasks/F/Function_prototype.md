[1]: http://rosettacode.org/wiki/Function_prototype

# [Function prototype][1]

There is no restriction on placement of prototype declarations. (Actually, we call them "stub declarations".) In fact, stub declarations are rarely needed in Perl 6 because post-declaration of functions is allowed, and normal [function declarations](http://design.perl6.org/S06.html#Subroutines_and_other_code_objects) do not bend the syntax the way they sometimes do in Perl 5.



Note that the <tt>...</tt> in all of these stub bodies is literally part of the declaration syntax.



A prototype declaration for a function that does not require arguments (and returns an Int):

```perl
sub foo ( --> Int) {...}
```


A prototype declaration for a function that requires two arguments.
Note that we can omit the variable name and just use the sigil in the stub, since we don't need to reference the argument until the actual definition of the routine. Also, unlike in Perl 5, a sigil like <tt>@</tt> defaults to binding a single positional argument.

```perl
sub foo (@, $ --> Int) {...}
```


A prototype declaration for a function that utilizes varargs after one required argument.
Note the "slurpy" star turns the <tt>@</tt> sigil into a parameter that accepts all the rest of the positional arguments.

```perl
sub foo ($, *@ --> Int) {...}
```


A prototype declaration for a function that utilizes optional arguments after one required argument. Optionality is conferred by either a question mark or a default:

```perl
sub foo ($, $?, $ = 42 --> Int) {...}
```


A prototype declaration for a function that utilizes named parameters:

```perl
sub foo ($, :$faster, :$cheaper --> Int) {...}
```


Example of prototype declarations for subroutines or procedures, which in Perl 6 is done simply by noting that nothing is returned:

```perl
sub foo ($, $ --> Nil) {...}
```


A routine may also slurp up all the named arguments that were not bound earlier in the signature:

```perl
sub foo ($, :$option, *% --> Int) {...}
```


A routine may make a named parameter mandatory using exclamation mark. (This is more useful in multi subs than in stubs though.)

```perl
sub foo ($, :$option! --> Int) {...}
```


A routine may unpack an <tt>Array</tt> automaticly. Here the first element is stored in a scalar and the rest in an <tt>Array</tt>. Other buildin types can be [unpacked](http://design.perl6.org/S06.html#Unpacking_array_parameters) as well.

```perl
sub foo ([$, @]) {...}
```


A routine may ask for a closure parameter to implement higher order functions. Typed or untyped signatures can be supplied.

```perl
sub foo (@, &:(Str --> Int)) {...}
```