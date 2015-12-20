[1]: http://rosettacode.org/wiki/Sort_disjoint_sublist

# [Sort disjoint sublist][1]

Using L-value slice of the array, and \`sort\` as a mutating method:

```perl
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
sub disjointSort( @values is rw , @indices is rw --> List ) {
   my @sortedValues = @values[ @indices ].sort ;
   for @indices.sort -> $insert {
      @values[ $insert ] = @sortedValues.shift ;
   }
   return @values ;
}
 
my @values = ( 7 , 6 , 5 , 4 , 3 , 2 , 1 , 0 ) ;
my @indices = ( 6 , 1 , 7 ) ;
my @sortedValues = disjointSort( @values , @indices ) ;
@sortedValues.perl.say ;
```


Output:


#### Output:
```
[7, 0, 5, 4, 3, 2, 1, 6]
```