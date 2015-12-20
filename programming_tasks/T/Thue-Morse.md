[1]: http://rosettacode.org/wiki/Thue-Morse

# [Thue-Morse][1]

```perl6
my @tm = 0, { @_.map: + ! * } ... *;
 
say @tm[^64].join;
```

#### Output:
```
0110100110010110100101100110100110010110011010010110100110010110
```