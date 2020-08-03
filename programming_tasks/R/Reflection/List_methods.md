[1]: https://rosettacode.org/wiki/Reflection/List_methods

# [Reflection/List methods][1]

You can get a list of an object's methods using `.^methods`, which is part of the [Meta Object Protocol](https://docs.perl6.org/type/Metamodel$COLON$COLONClassHOW).

Each is represented as a `Method` object that contains a bunch of info:

```raku
class Foo {
    method foo ($x)      { }
    method bar ($x, $y)  { }
    method baz ($x, $y?) { }
}
 
my $object = Foo.new;
 
for $object.^methods {
    say join ", ", .name, .arity, .count, .signature.gist
}
```

#### Output:
```
foo, 2, 2, (Foo $: $x, *%_)
bar, 3, 3, (Foo $: $x, $y, *%_)
baz, 2, 3, (Foo $: $x, $y?, *%_)
```