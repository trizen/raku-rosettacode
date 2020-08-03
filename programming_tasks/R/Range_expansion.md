[1]: https://rosettacode.org/wiki/Range_expansion

# [Range expansion][1]

```raku
sub range-expand (Str $range-description) {
    my token number { '-'? \d+ }
    my token range  { (<&number>) '-' (<&number>) }
 
    $range-description
        .split(',')
        .map({ .match(&range) ?? $0..$1 !! +$_ })
        .flat
}
 
say range-expand('-6,-3--1,3-5,7-11,14,15,17-20').join(', ');
```

#### Output:
```
-6, -3, -2, -1, 3, 4, 5, 7, 8, 9, 10, 11, 14, 15, 17, 18, 19, 20
```




Alternatively, using a grammar:

```raku
grammar RangeList {
    token TOP    { <term>* % ','    { make $<term>.map(*.made)       } }
    token term   { [<range>|<num>]  { make ($<num> // $<range>).made } }
    token range  { <num> '-' <num>  { make +$<num>[0] .. +$<num>[1]  } }
    token num    { '-'? \d+         { make +$/                       } }
}
 
say RangeList.parse('-6,-3--1,3-5,7-11,14,15,17-20').made.flat.join(', ');
```

#### Output:
```
-6, -3, -2, -1, 3, 4, 5, 7, 8, 9, 10, 11, 14, 15, 17, 18, 19, 20
```