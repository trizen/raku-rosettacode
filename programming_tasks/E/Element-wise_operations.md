[1]: https://rosettacode.org/wiki/Element-wise_operations

# [Element-wise operations][1]


Raku already implements this and other metaoperators as higher-order functions (cross, zip, reduce, triangle, etc.) that are usually accessed through a meta-operator syntactic sugar that is productive over all appropriate operators, including user-defined ones.  In this case, a dwimmy element-wise operator (generically known as a "hyper") is indicated by surrounding the operator with double angle quotes.  Hypers dwim on the pointy end with cyclic APL semantics as necessary.  You can turn the quote the other way to suppress dwimmery on that end.  In this case we could have used `»op»` instead of `«op»` since the short side is always on the right.

```perl
my @a =
    [1,2,3],
    [4,5,6],
    [7,8,9];

sub msay(@x) {
    say .map( { ($_%1) ?? $_.nude.join('/') !! $_ } ).join(' ') for @x;
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
