[1]: http://rosettacode.org/wiki/Break_OO_privacy

# [Break OO privacy][1]

We may call into the MOP (Meta-Object Protocol) via the `.^` operator, and the MOP knows all about the object, including any supposedly private bits. We ask for its attributes, find the correct one, and get its value.

```perl
class Foo {
    has $!shyguy = 42;
}
my Foo $foo .= new;
 
say $foo.^attributes.first('$!shyguy').get_value($foo);
```

#### Output:
```
42
```