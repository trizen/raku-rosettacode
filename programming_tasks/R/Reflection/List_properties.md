[1]: https://rosettacode.org/wiki/Reflection/List_properties

# [Reflection/List properties][1]

You can get a list of an object's attributes (instance variables) using `.^attributes`, which is part of the [Meta Object Protocol](https://docs.perl6.org/type/Metamodel$COLON$COLONClassHOW)..

Each is represented as an `Attribute` object that contains a bunch of info:

```perl
class Foo {
    has $!a = now;
    has Str $.b;
    has Int $.c is rw;
}
 
my $object = Foo.new: b => "Hello", c => 42;
 
for $object.^attributes {
    say join ", ", .name, .readonly, .container.^name, .get_value($object);
}
```

#### Output:
```
$!a, True, Any, Instant:1470517602.295992
$!b, True, Str, Hello
$!c, False, Int, 42
```


Public attributes (in this case, `$.b` and `$.c`) are really just attributes for which the compiler also auto-generates a method of the same name. See [Reflection/List_methods#Raku](https://rosettacode.org/wiki/Reflection/List_methods#Raku).