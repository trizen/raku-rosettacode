[1]: https://rosettacode.org/wiki/Cartesian_product_of_two_or_more_lists

# [Cartesian product of two or more lists][1]

The cross meta operator X will return the cartesian product of two lists. To apply the cross meta-operator to a variable number of lists, use the reduce cross meta operator [X].

```perl
# cartesian product of two lists using the X cross meta-operator
say (1, 2) X (3, 4);
say (3, 4) X (1, 2);
say (1, 2) X ( );
say ( )    X ( 1, 2 );
 
# cartesian product of variable number of lists using
# the [X] reduce cross meta-operator
say [X] (1776, 1789), (7, 12), (4, 14, 23), (0, 1);
say [X] (1, 2, 3), (30), (500, 100);
say [X] (1, 2, 3), (),   (500, 100);
```

#### Output:
```
((1 3) (1 4) (2 3) (2 4))
((3 1) (3 2) (4 1) (4 2))
()
()
((1776 7 4 0) (1776 7 4 1) (1776 7 14 0) (1776 7 14 1) (1776 7 23 0) (1776 7 23 1) (1776 12 4 0) (1776 12 4 1) (1776 12 14 0) (1776 12 14 1) (1776 12 23 0) (1776 12 23 1) (1789 7 4 0) (1789 7 4 1) (1789 7 14 0) (1789 7 14 1) (1789 7 23 0) (1789 7 23 1) (1789 12 4 0) (1789 12 4 1) (1789 12 14 0) (1789 12 14 1) (1789 12 23 0) (1789 12 23 1))
((1 30 500) (1 30 100) (2 30 500) (2 30 100) (3 30 500) (3 30 100))
()
```