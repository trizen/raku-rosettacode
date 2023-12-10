[1]: https://rosettacode.org/wiki/Sort_disjoint_sublist

# [Sort disjoint sublist][1]





### Inline



Using L-value slice of the array, and \`sort\` as a mutating method:

```perl
my @values  = 7, 6, 5, 4, 3, 2, 1, 0;
my @indices = 6, 1, 7;

@values[ @indices.sort ] .= sort;

say @values;
```

#### Output:
```
[7, 0, 5, 4, 3, 2, 1, 6]
```


### Iterative

```perl
sub disjointSort( @values, @indices --> List ) {
   my @sortedValues = @values[ @indices ].sort ;
   for @indices.sort -> $insert {
      @values[ $insert ] = @sortedValues.shift ;
   }
   return @values ;
}

my @values = ( 7 , 6 , 5 , 4 , 3 , 2 , 1 , 0 ) ;
my @indices = ( 6 , 1 , 7 ) ;
my @sortedValues = disjointSort( @values , @indices ) ;
say @sortedValues ;
```

#### Output:
```
[7, 0, 5, 4, 3, 2, 1, 6]
```
