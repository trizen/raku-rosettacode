[1]: https://rosettacode.org/wiki/A+B

# [A+B][1]

Short version with very little "line noise":

```raku
get.words.sum.say;
```


Reduction operator `[+]`, and `say` as a function:

```raku
say [+] get.words;
```


Long version:

```raku
my ($a, $b) = $*IN.get.split(" ");
say $a + $b;
```