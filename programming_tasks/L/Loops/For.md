[1]: https://rosettacode.org/wiki/Loops/For

# [Loops/For][1]



```perl
for ^5 {

	for 0..$_ {
		print "*";
	}
	
	print "\n";

}
```


or using only one for loop:

```perl
say '*' x $_ for 1..5;
```


or without using any loops at all:

```perl
([\~] "*" xx 5).join("\n").say;
```
