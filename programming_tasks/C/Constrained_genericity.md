[1]: http://rosettacode.org/wiki/Constrained_genericity

# [Constrained genericity][1]

```perl6
subset Eatable of Any where { .^can('eat') };
 
class Cake { method eat() {...} }
 
role FoodBox[Eatable ::T] {
    has T %.foodbox;
}
 
class Yummy does FoodBox[Cake] { }      # composes correctly
# class Yucky does FoodBox[Int] { }     # fails to compose
 
my Yummy $foodbox .= new;
say $foodbox.perl;
```
```text
Yummy.new(foodbox => {})
```