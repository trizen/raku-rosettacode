[1]: https://rosettacode.org/wiki/Sort_numbers_lexicographically

# [Sort numbers lexicographically][1]

```raku
sub lex (Int $n) { (1…$n).sort: ~* }
 
# TESTING
printf("%4d: [%s]\n", $_, .&lex.join: ',') for 13, 21, -22
```

#### Output:
```
  13: [1,10,11,12,13,2,3,4,5,6,7,8,9]
  21: [1,10,11,12,13,14,15,16,17,18,19,2,20,21,3,4,5,6,7,8,9]
 -22: [-1,-10,-11,-12,-13,-14,-15,-16,-17,-18,-19,-2,-20,-21,-22,-3,-4,-5,-6,-7,-8,-9,0,1]
```