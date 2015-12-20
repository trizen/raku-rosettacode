[1]: http://rosettacode.org/wiki/A+B

# [A+B][1]

```perl
say [+] get.words
```
```perl
$*IN.get.words.reduce(* + *).say
```
```perl
my ($a,$b) = $*IN.get.split(" ");
say $a + $b;
```