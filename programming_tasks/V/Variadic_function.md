[1]: https://rosettacode.org/wiki/Variadic_function

# [Variadic function][1]





If a subroutine has no formal parameters but mentions the variables `@_` or `%_` in its body, it will accept arbitrary positional or keyword arguments, respectively. You can even use both in the same function.

```perl
sub foo {
   .say for @_;
   say .key, ': ', .value for %_;
}

foo 1, 2, command => 'buckle my shoe',
    3, 4, order => 'knock at the door';
```


This prints:


```
1
2
3
4
command: buckle my shoe
order: knock at the door
```


Raku also supports slurpy arrays and hashes, which are formal parameters that consume extra positional and keyword arguments like `@_` and `%_`. You can make a parameter slurpy with the `*` twigil. This implementation of `&foo` works just like the last:

```perl
sub foo (*@positional, *%named) {
   .say for @positional;
   say .key, ': ', .value for %named;
}
```


Unlike in Perl 5, arrays and hashes aren't flattened automatically. Use the `|` operator to flatten:

```perl
foo |@ary, |%hsh;
```
