[1]: https://rosettacode.org/wiki/Inner_classes

# [Inner classes][1]

Raku supports nested classes, however they are really only useful for grouping units of behavior together into a namespace. There is no particular benefit as far as access to private methods or attributes that can't be achieved by non-nested classes. Access to inner classes must use fully qualified paths.

```perl
class Outer {
    has $.value is rw;
    method double { self.value ×= 2 }

    class Inner {
        has $.value is rw;
    }
}

# New Outer instance with a value of 42.
my $outer = Outer.new(:value(42));

say .^name, ' ', .value given $outer;
$outer.double;
say .^name, ' ', .value given $outer;

# New Inner instance with no value set. Note need to specify fully qualified name.
my $inner = Outer::Inner.new;

# Set a value after the fact.
$inner.value = 16;

# There is no way for the Inner object to call the Outer .double method.
# It is a separate, distinct class, just in a funny namespace.
say .^name, ' ', .value given $inner;
$inner.value /=2;
say .^name, ' ', .value given $inner;
```

#### Output:
```
Outer 42
Outer 84
Outer::Inner 16
Outer::Inner 8
```
