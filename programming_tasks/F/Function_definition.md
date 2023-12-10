[1]: https://rosettacode.org/wiki/Function_definition

# [Function definition][1]


Without a signature:

```perl
sub multiply { return @_[0] * @_[1]; }
```


The return is optional on the final statement, since the last expression would return its value anyway.  The final semicolon in a block is also optional.
(Beware that a subroutine without an explicit signature, like this one, magically becomes variadic (rather than nullary) only if `@_` or `%_` appear in the body.)  In fact, we can define the variadic version explicitly, which still works for two arguments:

```perl
sub multiply { [*] @_ }
```


With formal parameters and a return type:

```perl
sub multiply (Rat $a, Rat $b --> Rat) { $a * $b }
```


Same thing:

```perl
my Rat sub multiply (Rat $a, Rat $b) { $a * $b }
```


It is possible to define a function in "lambda" notation and then bind that into a scope, in which case it works like any function:

```perl
my &multiply := -> $a, $b { $a * $b };
```


Another way to write a lambda is with internal placeholder parameters:

```perl
my &multiply := { $^a * $^b };
```


(And, in fact, our original `@_` above is just a variadic self-declaring placeholder argument.  And the famous Perl "topic", `$_`, is just a self-declared parameter to a unary block.)



You may also curry both built-in and user-defined operators by supplying a `*` (known as "whatever") in place of the argument that is *not* to be curried:

```perl
my &multiply := * * *;
```


This is not terribly readable in this case due to the visual confusion between the whatever star and the multiplication operator, but Perl knows when it's expecting terms instead of infixes, so only the middle star is multiplication.
It tends to work out much better with other operators.  In particular, you may
curry a cascade of methods with only the original invocant missing:

```perl
@list.grep( *.substr(0,1).lc.match(/<[0..9 a..f]>/) )
```


This is equivalent to:

```perl
@list.grep( -> $obj { $obj.substr(0,1).lc.match(/<[0..9 a..f]>/) } )
```
