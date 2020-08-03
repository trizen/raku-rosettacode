[1]: https://rosettacode.org/wiki/Loops/For

# [Loops/For][1]

```raku
for ^5 {
 
	for 0..$_ {
		print "*";
	}
 
	print "\n";
 
}
```


or using only one for loop:

```raku
say '*' x $_ for 1..5;
```


or without using any loops at all:

```raku
([\~] "*" xx 5).join("\n").say;
```