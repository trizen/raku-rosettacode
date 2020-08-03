[1]: https://rosettacode.org/wiki/Anonymous_recursion

# [Anonymous recursion][1]

In addition to the methods in the [Perl](https://rosettacode.org/wiki/Perl) entry above, and the Y-combinator described in [Y_combinator](https://rosettacode.org/wiki/Y_combinator), you may also refer to an anonymous block or function from the inside:

```perl
sub fib($n) {
    die "Naughty fib" if $n < 0;
    return {
        $_ < 2
            ?? $_
            !!  &?BLOCK($_-1) + &?BLOCK($_-2);
    }($n);
}
 
say fib(10);
```


However, using any of these methods is insane, when Perl 6 provides a sort of inside-out combinator that lets you define lazy infinite constants, where the demand for a particular value is divorced from dependencies on more primitive values. This operator, known as the sequence operator, does in a sense provide anonymous recursion to a closure that refers to more primitive values.

```perl
constant @fib = 0, 1, *+* ... *;
say @fib[10];
```


Here the closure, `*+*`, is just a quick way to write a lambda, `-> $a, $b { $a + $b }`. The sequence operator implicitly maps the two arguments to the -2nd and -1st elements of the sequence. So the sequence operator certainly applies an anonymous lambda, but whether it's recursion or not depends on whether you view a sequence as iteration or as simply a convenient way of memoizing a recursion. Either view is justifiable.



At this point someone may complain that the solution is doesn't fit the specified task because the sequence operator doesn't do the check for negative. True, but the sequence operator is not the whole of the solution; this check is supplied by the subscripting operator itself when you ask for `@fib[-1]`. Instead of scattering all kinds of arbitrary boundary conditions throughout your functions, the sequence operator maps them quite naturally to the boundary of definedness at the start of a list.