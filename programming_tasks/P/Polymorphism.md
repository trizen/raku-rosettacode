[1]: https://rosettacode.org/wiki/Polymorphism

# [Polymorphism][1]

All appropriate constructors, initializers, accessors, and destructors are provided by default, but may be explicitly declared for flexibility.
To create only readonly accessors for better encapsulation, leave out all the "is rw" traits.
Here we demonstrate that accessors can behave like variables and may be assigned.

```raku
class Point {
    has Real $.x is rw = 0;
    has Real $.y is rw = 0;
    method Str { $.perl }
}
 
class Circle {
    has Point $.p is rw = Point.new;
    has Real $.r is rw = 0;
    method Str { $.perl }
}
 
my $c = Circle.new(p => Point.new(x => 1, y => 2), r => 3);
say $c;
$c.p.x = (-10..10).pick;
$c.p.y = (-10..10).pick;
$c.r   = (0..10).pick;
say $c;
```


In this case we define the Str coercion method polymorphically, which is used by say or print to format the contents of the object.
We could also have defined print methods directly.
We could have factored this method out to a common role and composed it into each class.
We could also have defined multi subs outside of the class, like this:

```raku
multi print (Point $p) { $p.perl.print }
multi print (Circle $c) { $c.perl.print }
```