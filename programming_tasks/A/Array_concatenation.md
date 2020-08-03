[1]: https://rosettacode.org/wiki/Array_concatenation

# [Array concatenation][1]

```raku
# the prefix:<|> operator (called "slip") can be used to interpolate arrays into a list:
sub cat-arrays(@a, @b) { 
	|@a, |@b 
}
 
my @a1 = (1,2,3);
my @a2 = (2,3,4);
cat-arrays(@a1,@a2).join(", ").say;
```

#### Output:
```
1, 2, 3, 2, 3, 4
```