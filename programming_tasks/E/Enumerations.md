[1]: http://rosettacode.org/wiki/Enumerations

# [Enumerations][1]

```perl6
enum Fruit <Apple Banana Cherry>; # Numbered 0 through 2.
 
enum ClassicalElement (
    Earth => 5,
    'Air', # Gets the value 6.
    Fire => 'hot',
    Water => 'wet'
);
```