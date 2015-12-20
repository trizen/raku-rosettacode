[1]: http://rosettacode.org/wiki/Loops/For

# [Loops/For][1]

```perl6
for ^5 {
 
	for 0..$_ {
		print "*";
	}
 
	print "\n";
 
}
```


or using only one for loop:

```perl6
say '*' x $_ for 1..5;
```


or without using any loops at all:

```perl6
([\~] "*" xx 5).join("\n").say;
```