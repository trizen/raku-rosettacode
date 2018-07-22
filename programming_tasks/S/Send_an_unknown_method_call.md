[1]: https://rosettacode.org/wiki/Send_an_unknown_method_call

# [Send an unknown method call][1]

Just for the fun of it, we'll mix in an anonymous role into an integer instead of defining a class.

```perl
my $object = 42 but role { method add-me($x) { self + $x } }
my $name = 'add-me';
say $object."$name"(5);  # 47
```


The double quotes are required, by the way; without them the variable would be interpreted as a hard ref to a method.