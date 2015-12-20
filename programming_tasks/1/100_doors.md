[1]: http://rosettacode.org/wiki/100_doors

# [100 doors][1]

```perl6
my @doors = False xx 101;
 
(.=not for @doors[0, $_ ... 100]) for 1..100;
 
say "Door $_ is ", <closed open>[ @doors[$_] ] for 1..100;
```


*optimized*

```perl6
say "Door $_ is open" for map {$^n ** 2}, 1..10;
```


Here's a version using the cross meta-operator instead of a map:

```perl6
 say "Door $_ is open" for 1..10 X** 2;
```


This one prints both opened and closed doors:

```perl6
say "Door $_ is ", <closed open>[.sqrt == .sqrt.floor] for 1..100;
```