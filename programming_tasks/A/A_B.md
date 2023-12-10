[1]: https://rosettacode.org/wiki/A%2BB

# [A+B][1]





Short version with very little "line noise":

```perl
get.words.sum.say;
```


Reduction operator `[+]`, and `say` as a function:

```perl
say [+] get.words;
```


Long version:

```perl
my ($a, $b) = $*IN.get.split(" ");
say $a + $b;
```
