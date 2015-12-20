[1]: http://rosettacode.org/wiki/A+B

# [A+B][1]

```perl6
say [+] get.words
```
```perl6
$*IN.get.words.reduce(* + *).say
```
```perl6
my ($a,$b) = $*IN.get.split(" ");
say $a + $b;
```