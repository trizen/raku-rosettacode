[1]: http://rosettacode.org/wiki/Find_limit_of_recursion

# [Find limit of recursion][1]

Maximum recursion depth is memory dependent. Values in excess of 1 million are easily achieved.

```perl
my $x = 0;
recurse;
 
sub recurse () {
   ++$x;
   say $x if $x %% 1_000_000;   
   recurse;
}
```


When manually terminated memory use was on the order of 4Gb:


#### Output:
```
1000000
2000000
3000000
4000000
5000000
6000000
7000000
8000000
9000000
10000000
^C
```