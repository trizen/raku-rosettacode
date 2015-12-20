[1]: http://rosettacode.org/wiki/Maximum_triangle_path_sum

# [Maximum triangle path sum][1]

The <tt>Z+</tt> and <tt>Zmax</tt> are examples of the zipwith metaoperator. We ought to be able to use <tt>[Z+]=</tt> as an assignment operator here, but rakudo has a bug. Note also we can use the <tt>Zmax</tt> metaoperator form because <tt>max</tt> is define as an infix in Perl 6.

```perl6
my @rows = slurp("triangle.txt").lines.map: { [.words] }
 
while @rows > 1 {
    my @last := @rows.pop;
    @rows[*-1] = (@rows[*-1][] Z+ (@last Zmax @last[1..*])).List;
}
 
say @rows;
```

#### Output:
```
1320
```


Here's a more FPish version with the same output.



We define our own operator and the use it in the reduction metaoperator form, <tt>[op]</tt>, which turns any infix into a list operator.

```perl6
sub infix:<op>(@a,@b) { (@a Zmax @a[1..*]) Z+ @b }
 
say [op] slurp("triangle.txt").lines.reverse.map: { [.words] }
```


Instead of using reverse, one could also define the op as right-associative.

```perl6
sub infix:<op>(@a,@b) is assoc('right') { @a Z+ (@b Zmax @b[1..*]) }
 
say [op] slurp("triangle.txt").lines.map: { [.words] }
```