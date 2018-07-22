[1]: https://rosettacode.org/wiki/Inheritance/Single

# [Inheritance/Single][1]

```perl
class Animal {}
class Dog is Animal {}
class Cat is Animal {}
class Lab is Dog {}
class Collie is Dog {}
 
say Collie.^parents;     # undefined type object
say Collie.new.^parents; # instantiated object
```

#### Output:
```
((Dog) (Animal))
((Dog) (Animal))
```


The `.^parents` notation indicates a method call to the object's metaobject rather than to the object itself.