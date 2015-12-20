[1]: http://rosettacode.org/wiki/Sort_disjoint_sublist

# [Sort disjoint sublist][1]

Using L-value slice of the array, and \`sort\` as a mutating method:

```perl6
my @values  = 7, 6, 5, 4, 3, 2, 1, 0;
my @indices = 6, 1, 7;
 
@values[ @indices.sort ] .= sort;
 
@values.perl.say;
```

#### Output:
```
[7, 0, 5, 4, 3, 2, 1, 6]
```
```perl
sub disjointSort( @[values](http://perldoc.perl.org/functions/values.html) is rw , @indices is rw --> List ) {
   my @sortedValues = @[values](http://perldoc.perl.org/functions/values.html)[ @indices ].[sort](http://perldoc.perl.org/functions/sort.html) ;
   for @indices.[sort](http://perldoc.perl.org/functions/sort.html) -> $insert {
      @[values](http://perldoc.perl.org/functions/values.html)[ $insert ] = @sortedValues.[shift](http://perldoc.perl.org/functions/shift.html) ;
   }
   [return](http://perldoc.perl.org/functions/return.html) @[values](http://perldoc.perl.org/functions/values.html) ;
}
 
my @[values](http://perldoc.perl.org/functions/values.html) = ( 7 , 6 , 5 , 4 , 3 , 2 , 1 , 0 ) ;
my @indices = ( 6 , 1 , 7 ) ;
my @sortedValues = disjointSort( @[values](http://perldoc.perl.org/functions/values.html) , @indices ) ;
@sortedValues.perl.say ;
```


Output:


#### Output:
```
[7, 0, 5, 4, 3, 2, 1, 6]
```