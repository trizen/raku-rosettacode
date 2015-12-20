[1]: http://rosettacode.org/wiki/Order_two_numerical_lists

# [Order two numerical lists][1]

This is already a built-in comparison operator.

```perl
my @a = <1 2 4>;
my @b = <1 2 4>;
say @a," before ",@b," = ", @a before @b;
 
@a = <1 2 4>;
@b = <1 2>;
say @a," before ",@b," = ", @a before @b;
 
@a = <1 2>;
@b = <1 2 4>;
say @a," before ",@b," = ", @a before @b;
 
for 1..10 {
    my @a = (^100).roll((2..3).pick);
    my @b = @a.map: { Bool.pick ?? $_ !! (^100).roll((0..2).pick) }
    say @a," before ",@b," = ", @a before @b;
}
```

#### Output:
```
1 2 4 before 1 2 4 = False
1 2 4 before 1 2 = False
1 2 before 1 2 4 = True
63 52 before 0 52 = False
17 75 24 before 31 75 24 = True
43 32 before 43 32 = False
73 84 before 2 84 = False
73 92 before 40 24 46 = False
16 24 before 41 24 = True
9 12 22 before 9 12 32 67 = True
81 23 before 81 23 = False
55 53 1 before 55 62 83 = True
20 40 51 before 20 17 78 34 = False
```