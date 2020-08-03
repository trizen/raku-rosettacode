[1]: https://rosettacode.org/wiki/Element-wise_operations

# [Element-wise operations][1]

```raku
my @a =
    [1,2,3],
    [4,5,6],
    [7,8,9];
 
sub msay(@x) {
    for @x -> @row {
        print ' ', $_%1 ?? $_.nude.join('/') !! $_ for @row;
        say '';
    }
    say '';
}
 
msay @a «+» @a;
msay @a «-» @a;
msay @a «*» @a;
msay @a «/» @a;
msay @a «+» [1,2,3];
msay @a «-» [1,2,3];
msay @a «*» [1,2,3];
msay @a «/» [1,2,3];
msay @a «+» 2;
msay @a «-» 2;
msay @a «*» 2;
msay @a «/» 2;
 
# In addition to calling the underlying higher-order functions directly, it's possible to name a function.
 
sub infix:<M+> (\l,\r) { l <<+>> r }
 
msay @a M+ @a;
msay @a M+ [1,2,3];
msay @a M+ 2;
 
```

#### Output:
```
 2 4 6
 8 10 12
 14 16 18

 0 0 0
 0 0 0
 0 0 0

 1 4 9
 16 25 36
 49 64 81

 1 1 1
 1 1 1
 1 1 1

 2 3 4
 6 7 8
 10 11 12

 0 1 2
 2 3 4
 4 5 6

 1 2 3
 8 10 12
 21 24 27

 1 2 3
 2 5/2 3
 7/3 8/3 3

 3 4 5
 6 7 8
 9 10 11

 -1 0 1
 2 3 4
 5 6 7

 2 4 6
 8 10 12
 14 16 18

 1/2 1 3/2
 2 5/2 3
 7/2 4 9/2

 2 4 6
 8 10 12
 14 16 18

 2 3 4
 6 7 8
 10 11 12

 3 4 5
 6 7 8
 9 10 11
```