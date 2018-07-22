[1]: https://rosettacode.org/wiki/Catalan_numbers

# [Catalan numbers][1]

The recursive formulas are easily written into a constant array, either:

```perl
constant Catalan = 1, { [+] @_ Z* @_.reverse } ... *;
```


or

```perl
constant Catalan = 1, |[\*] (2, 6 ... *) Z/ 2 .. *;
 
 
# In both cases, the sixteen first values can be seen with:
.say for Catalan[^16];
```

#### Output:
```
1
1
2
5
14
42
132
429
1430
4862
16796
58786
208012
742900
2674440
```